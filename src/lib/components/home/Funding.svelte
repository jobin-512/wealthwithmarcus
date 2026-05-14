<script>
// @ts-nocheck

  import { onMount } from 'svelte';
  
  let sectionRef;
  let headlineWords = [];
  let cards = [];
  let lineRef;
  let lightRef;
  let ctaRef;
  let orb1, orb2;
  
  const steps = [
    {
      num: '01',
      title: 'Take 1-minute Survey',
      desc: 'Share your goals and current situation so we can pinpoint the ideal funding strategy for your needs.',
      icon: 'M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2m-3 7h3m-3 4h3m-6-4h.01M9 16h.01'
    },
    {
      num: '02',
      title: 'Book a Discovery Call',
      desc: 'Speak one-on-one with Marcus to review tailored options and map out a clear path to approval.',
      icon: 'M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z'
    },
    {
      num: '03',
      title: 'Get Funded in 5-days',
      desc: 'Leverage our network of 90+ lenders for fast approvals and rapid access to capital.',
      icon: 'M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z'
    }
  ];
  
  // ─── 3D Tilt + Glare ───
  function onCardMove(e, card) {
    const rect = card.getBoundingClientRect();
    const x = e.clientX - rect.left;
    const y = e.clientY - rect.top;
    const cx = rect.width / 2;
    const cy = rect.height / 2;
    const rx = ((y - cy) / cy) * -8;
    const ry = ((x - cx) / cx) * 8;
    card.style.transform = `perspective(1000px) rotateX(${rx}deg) rotateY(${ry}deg) scale3d(1.03,1.03,1.03)`;
    const glare = card.querySelector('.glare');
    if (glare) glare.style.background = `radial-gradient(circle at ${x}px ${y}px, rgba(255,255,255,0.12), transparent 50%)`;
  }
  function onCardLeave(card) {
    card.style.transform = 'perspective(1000px) rotateX(0) rotateY(0) scale3d(1,1,1)';
    const glare = card.querySelector('.glare');
    if (glare) glare.style.background = 'transparent';
  }
  
  // ─── Magnetic CTA ───
  function onMagneticMove(e) {
    if (!ctaRef) return;
    const rect = ctaRef.getBoundingClientRect();
    const dx = (e.clientX - rect.left - rect.width / 2) * 0.25;
    const dy = (e.clientY - rect.top - rect.height / 2) * 0.25;
    ctaRef.style.transform = `translate(${dx}px, ${dy}px)`;
  }
  function onMagneticLeave() {
    if (ctaRef) ctaRef.style.transform = 'translate(0,0)';
  }
  
  onMount(async () => {
    const gsap = (await import('gsap')).default;
    const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');
    gsap.registerPlugin(ScrollTrigger);
    
    const ctx = gsap.context(() => {
      // Ambient orbs float
      gsap.to(orb1, { y: -40, x: 30, duration: 7, repeat: -1, yoyo: true, ease: 'sine.inOut' });
      gsap.to(orb2, { y: 30, x: -20, duration: 9, repeat: -1, yoyo: true, ease: 'sine.inOut' });
      
      // Headline 3D word reveal
      gsap.from(headlineWords.filter(Boolean), {
        y: 90,
        rotateX: -75,
        opacity: 0,
        duration: 1.1,
        stagger: 0.07,
        ease: 'power3.out',
        scrollTrigger: { trigger: sectionRef, start: 'top 72%' }
      });
      
      // SVG connecting line draw
      if (lineRef) {
        const len = lineRef.getTotalLength();
        gsap.set(lineRef, { strokeDasharray: len, strokeDashoffset: len });
        gsap.to(lineRef, {
          strokeDashoffset: 0,
          duration: 1.8,
          ease: 'power2.inOut',
          scrollTrigger: { trigger: cards[0], start: 'top 82%' }
        });
      }
      
      // Traveling light along the line
      if (lightRef) {
        gsap.fromTo(lightRef, 
          { attr: { offset: '0%' } },
          { attr: { offset: '100%' }, duration: 2, ease: 'power2.inOut',
            scrollTrigger: { trigger: cards[0], start: 'top 75%', toggleActions: 'play none none reverse' }
          }
        );
      }
      
      // Cards stagger up with slight rotateX
      gsap.from(cards.filter(Boolean), {
        y: 140,
        opacity: 0,
        rotateX: 18,
        duration: 1,
        stagger: 0.18,
        ease: 'power3.out',
        scrollTrigger: { trigger: cards[0], start: 'top 88%' }
      });
      
      // Icon pop
      gsap.from('.step-icon', {
        scale: 0,
        duration: 0.6,
        stagger: 0.18,
        ease: 'back.out(1.7)',
        scrollTrigger: { trigger: cards[0], start: 'top 80%' }
      });
    }, sectionRef);
    
    return () => ctx.revert();
  });
</script>

<section 
  bind:this={sectionRef}
  class="relative overflow-hidden bg-background py-32 lg:py-44"
  on:mousemove={onMagneticMove}
  role="presentation"
>
  <!-- ── Ambient Background ── -->
  <div class="pointer-events-none absolute inset-0">
    <!-- svelte-ignore element_invalid_self_closing_tag -->
    <div bind:this={orb1} class="absolute -left-40 top-20 h-[600px] w-[600px] rounded-full bg-accent/[0.035] blur-[130px]" />
    <div bind:this={orb2} class="absolute -right-40 bottom-20 h-[500px] w-[500px] rounded-full bg-accent/[0.025] blur-[110px]" />
    <!-- svelte-ignore element_invalid_self_closing_tag -->
    <div class="absolute inset-0 bg-[url('data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHdpZHRoPSIzMDAiIGhlaWdodD0iMzAwIj48ZmlsdGVyIGlkPSJhIiB4PSIwIiB5PSIwIj48ZmVUdXJidWxlbmNlIGJhc2VGcmVxdWVuY3k9Ii43NSIgc3RpdGNoVGlsZXM9InN0aXRjaCIgdHlwZT0iZnJhY3RhbE5vaXNlIi8+PGZlQ29sb3JNYXRyaXggdHlwZT0ic2F0dXJhdGUiIHZhbHVlcz0iMCIvPjwvZmlsdGVyPjxyZWN0IHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiIGZpbHRlcj0idXJsKCNhKSIgb3BhY2l0eT0iMC4wMyIvPjwvc3ZnPg==')] opacity-30" />
  </div>

  <!-- ── Top Hairline ── -->
  <div class="absolute top-0 left-0 right-0 h-px bg-gradient-to-r from-transparent via-border to-transparent" />

  <div class="relative mx-auto max-w-7xl px-6">
    
    <!-- ═══════ Editorial Header ═══════ -->
    <div class="mb-24 grid grid-cols-1 items-end gap-14 lg:grid-cols-2 lg:gap-24">
      <div>
        <p class="mb-5 font-mono text-xs uppercase tracking-[0.3em] text-muted">How It Works</p>
        <h2 class="font-serif text-[clamp(2.5rem,5vw,4.5rem)] font-medium leading-[1.05] tracking-tight text-foreground" style="perspective: 1000px;">
          {#each 'Unlock Your Mortgage Potential with Marcus —'.split(' ') as word, i}
            <span bind:this={headlineWords[i]} class="mr-[0.25em] inline-block origin-bottom">{word}</span>
          {/each}
          <br class="hidden sm:block" />
          <span class="inline-block italic text-accent">Experience the Difference Today!</span>
        </h2>
      </div>

      <div class="lg:pb-2">
        <p class="text-lg leading-relaxed text-muted">
          With access to over <span class="text-foreground font-medium">90 lenders</span>, Marcus ensures you find the best mortgage options tailored to your needs. Enjoy fast approvals without the pressure, making your home financing journey smoother.
        </p>
        <p class="mt-4 text-lg leading-relaxed text-muted">
          Trust in a seasoned expert who prioritizes your financial goals and peace of mind.
        </p>
        
        <!-- Magnetic CTA -->
        <div class="mt-10 inline-block" on:mouseleave={onMagneticLeave} role="button" tabindex="0">
          <a 
            bind:this={ctaRef}
            href="/contact" 
            class="relative inline-flex items-center gap-3 overflow-hidden rounded-full bg-accent px-8 py-4 text-sm font-semibold text-background transition-shadow duration-500 hover:shadow-[0_0_50px_-12px_rgba(197,160,89,0.45)]"
            style="transition: transform 0.3s cubic-bezier(0.23,1,0.32,1); will-change: transform;"
          >
            <span class="relative z-10">Get Started Today</span>
            <svg class="relative z-10 h-4 w-4" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2.5">
              <path stroke-linecap="round" stroke-linejoin="round" d="M17 8l4 4m0 0l-4 4m4-4H3" />
            </svg>
            <!-- Shine sweep -->
            <div class="absolute inset-0 -translate-x-full bg-gradient-to-r from-transparent via-white/20 to-transparent transition-transform duration-700 group-hover:translate-x-full" />
          </a>
        </div>
      </div>
    </div>

    <!-- ═══════ Steps Grid ═══════ -->
    <div class="relative">
      
      <!-- Desktop Connecting Line -->
      <svg class="absolute left-[16.66%] right-[16.66%] top-[88px] hidden h-[2px] w-[66.66%] overflow-visible lg:block" preserveAspectRatio="none">
        <defs>
          <linearGradient id="travel" x1="0%" y1="0%" x2="100%" y2="0%">
            <stop offset="0%" stop-color="rgba(197,160,89,0)" />
            <stop bind:this={lightRef} offset="0%" stop-color="rgba(197,160,89,0.8)" />
            <stop offset="4%" stop-color="rgba(197,160,89,0)" />
          </linearGradient>
        </defs>
        <line 
          bind:this={lineRef}
          x1="0" y1="1" x2="100%" y2="1" 
          stroke="url(#travel)" 
          stroke-width="2" 
          stroke-linecap="round"
        />
        <!-- Glow filter -->
        <line x1="0" y1="1" x2="100%" y2="1" stroke="rgba(197,160,89,0.15)" stroke-width="6" stroke-linecap="round" />
      </svg>

      <div class="grid grid-cols-1 gap-6 lg:grid-cols-3 lg:gap-10">
        {#each steps as step, i}
          <!-- svelte-ignore a11y_no_static_element_interactions -->
          <!-- svelte-ignore event_directive_deprecated -->
          <div
            bind:this={cards[i]}
            class="group relative rounded-2xl border border-border bg-surface/[0.35] p-8 backdrop-blur-2xl transition-all duration-500 ease-out sm:p-10"
            style="transform-style: preserve-3d; will-change: transform;"
            on:mousemove={(e) => onCardMove(e, cards[i])}
            on:mouseleave={() => onCardLeave(cards[i])}
          >
            <!-- Glare overlay -->
            <div class="glare pointer-events-none absolute inset-0 rounded-2xl transition-all duration-100" />
            
            <!-- Inner subtle gradient on hover -->
            <div class="pointer-events-none absolute inset-0 rounded-2xl bg-gradient-to-b from-accent/[0.07] to-transparent opacity-0 transition-opacity duration-500 group-hover:opacity-100" />
            
            <!-- Top row -->
            <div class="relative mb-10 flex items-center justify-between">
              <span class="font-mono text-[5.5rem] font-light leading-none text-accent/[0.15] transition-colors duration-500 group-hover:text-accent/30 select-none">
                {step.num}
              </span>
              
              <div class="step-icon flex h-14 w-14 items-center justify-center rounded-full border border-border bg-background/60 shadow-lg shadow-black/20 transition-all duration-500 group-hover:border-accent/40 group-hover:shadow-[0_0_30px_-5px_rgba(197,160,89,0.25)]">
                <svg class="h-5 w-5 text-muted transition-colors duration-300 group-hover:text-accent" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="1.5">
                  <path stroke-linecap="round" stroke-linejoin="round" d={step.icon} />
                </svg>
              </div>
            </div>
            
            <!-- Content -->
            <h3 class="relative mb-4 font-serif text-2xl font-medium text-foreground transition-colors duration-300 group-hover:text-accent">
              {step.title}
            </h3>
            <p class="relative leading-relaxed text-muted">
              {step.desc}
            </p>

            <!-- Mobile arrow -->
            {#if i < 2}
              <div class="mt-8 flex justify-center lg:hidden">
                <div class="flex h-10 w-10 items-center justify-center rounded-full border border-border animate-bounce">
                  <svg class="h-4 w-4 text-accent" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                    <path stroke-linecap="round" stroke-linejoin="round" d="M19 14l-7 7m0 0l-7-7m7 7V3" />
                  </svg>
                </div>
              </div>
            {/if}
          </div>
        {/each}
      </div>
    </div>
  </div>
</section>