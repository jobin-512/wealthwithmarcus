<script>
// @ts-nocheck
  import { onMount } from 'svelte';

  let sectionRef;
  let headlineWords = $state([]);
  let cards = $state([]);
  let lineRef;
  let lineGlowRef;
  let ctaRef;
  let orb1, orb2;
  let canvasRef;
  let dot1, dot2, dot3;
  let counterEls = $state([]);
  let tagRef;
  let bodyRef;
  let scanlineRef;

  const steps = [
    {
      num: '01',
      title: 'Take 1-minute Survey',
      desc: 'Share your goals and current situation so we can pinpoint the ideal funding strategy for your needs.',
      icon: 'M9 5H7a2 2 0 00-2 2v12a2 2 0 002 2h10a2 2 0 002-2V7a2 2 0 00-2-2h-2M9 5a2 2 0 002 2h2a2 2 0 002-2M9 5a2 2 0 012-2h2a2 2 0 012 2m-3 7h3m-3 4h3m-6-4h.01M9 16h.01',
      stat: 60, suffix: 's', statLabel: 'avg. time'
    },
    {
      num: '02',
      title: 'Book a Discovery Call',
      desc: 'Speak one-on-one with Marcus to review tailored options and map out a clear path to approval.',
      icon: 'M8 7V3m8 4V3m-9 8h10M5 21h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v12a2 2 0 002 2z',
      stat: 30, suffix: 'min', statLabel: 'session'
    },
    {
      num: '03',
      title: 'Get Funded in 5-days',
      desc: 'Leverage our network of 90+ lenders for fast approvals and rapid access to capital.',
      icon: 'M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 0118 0z',
      stat: 5, suffix: 'd', statLabel: 'to funded'
    }
  ];

  function onCardMove(e, card) {
    const rect = card.getBoundingClientRect();
    const x = e.clientX - rect.left;
    const y = e.clientY - rect.top;
    const cx = rect.width / 2, cy = rect.height / 2;
    const rx = ((y - cy) / cy) * -10;
    const ry = ((x - cx) / cx) * 10;
    card.style.transform = `perspective(1100px) rotateX(${rx}deg) rotateY(${ry}deg) scale3d(1.035,1.035,1.035)`;
    const glare = card.querySelector('.glare');
    if (glare) glare.style.background = `radial-gradient(circle at ${x}px ${y}px, rgba(197,160,89,0.13), transparent 55%)`;
  }
  function onCardLeave(card) {
    card.style.transform = 'perspective(1100px) rotateX(0) rotateY(0) scale3d(1,1,1)';
    const glare = card.querySelector('.glare');
    if (glare) glare.style.background = 'transparent';
  }

  function onMagneticMove(e) {
    if (!ctaRef) return;
    const rect = ctaRef.getBoundingClientRect();
    const dx = (e.clientX - rect.left - rect.width / 2) * 0.28;
    const dy = (e.clientY - rect.top - rect.height / 2) * 0.28;
    ctaRef.style.transform = `translate(${dx}px,${dy}px)`;
  }
  function onMagneticLeave() {
    if (ctaRef) ctaRef.style.transform = 'translate(0,0)';
  }

  onMount(async () => {
    const gsap = (await import('gsap')).default;
    const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');
    const { SplitText } = await import('gsap/dist/SplitText');
    const { DrawSVGPlugin } = await import('gsap/dist/DrawSVGPlugin');
    gsap.registerPlugin(ScrollTrigger, SplitText, DrawSVGPlugin);

    // ── Particle field ────────────────────────────────────────────────
    const canvas = canvasRef;
    const ctx = canvas.getContext('2d');
    let W, H;
    function resizeCanvas() {
      W = canvas.offsetWidth; H = canvas.offsetHeight;
      canvas.width = W; canvas.height = H;
    }
    resizeCanvas();
    window.addEventListener('resize', resizeCanvas);

    const GOLD = 'rgba(197,160,89,';
    const pts = Array.from({ length: 90 }, () => ({
      x: Math.random() * (W || 1200), y: Math.random() * (H || 800),
      vx: (Math.random() - 0.5) * 0.28, vy: (Math.random() - 0.5) * 0.28,
      r: Math.random() * 1.4 + 0.3,
      pa: Math.random() * Math.PI * 2,
      ps: (Math.random() * 0.005 + 0.002) * (Math.random() > 0.5 ? 1 : -1),
      a: 0
    }));

    let scrollProg = 0;
    ScrollTrigger.create({
      trigger: sectionRef, start: 'top bottom', end: 'bottom top',
      onUpdate: s => { scrollProg = s.progress; }
    });

    let raf;
    function tick() {
      ctx.clearRect(0, 0, W, H);
      pts.forEach((p, i) => {
        p.pa += p.ps;
        p.a = (Math.sin(p.pa) + 1) / 2 * 0.55 + 0.04;
        p.x += p.vx + scrollProg * 0.3;
        p.y += p.vy - scrollProg * 0.6;
        if (p.x < 0) p.x = W; if (p.x > W) p.x = 0;
        if (p.y < 0) p.y = H; if (p.y > H) p.y = 0;
        for (let j = i + 1; j < pts.length; j++) {
          const dx = p.x - pts[j].x, dy = p.y - pts[j].y;
          const d = Math.sqrt(dx * dx + dy * dy);
          if (d < 110) {
            ctx.beginPath();
            ctx.strokeStyle = GOLD + (0.07 * (1 - d / 110)) + ')';
            ctx.lineWidth = 0.5;
            ctx.moveTo(p.x, p.y); ctx.lineTo(pts[j].x, pts[j].y);
            ctx.stroke();
          }
        }
        ctx.beginPath();
        ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
        ctx.fillStyle = GOLD + p.a + ')';
        ctx.fill();
      });
      raf = requestAnimationFrame(tick);
    }
    tick();

    // ── Orbs float ───────────────────────────────────────────────────
    gsap.to(orb1, { y: -60, x: 35, duration: 8, repeat: -1, yoyo: true, ease: 'sine.inOut' });
    gsap.to(orb2, { y: 40, x: -25, duration: 10, repeat: -1, yoyo: true, ease: 'sine.inOut' });

    // ── Scanline ─────────────────────────────────────────────────────
    gsap.fromTo(scanlineRef,
      { top: '0%' }, { top: '100%', duration: 6, repeat: -1, ease: 'none' }
    );

    // ── Tag fade ─────────────────────────────────────────────────────
    gsap.from(tagRef, {
      opacity: 0, y: 16, duration: 0.7, ease: 'power3.out',
      scrollTrigger: { trigger: sectionRef, start: 'top 75%', once: true }
    });

    // ── Headline scramble ─────────────────────────────────────────────
    const CHARS = '!<>—_\\/[]{}=+*^?#ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
    const validWords = headlineWords.filter(Boolean);

    function scrambleWord(el, final, delay) {
      let iter = 0, max = 10;
      setTimeout(() => {
        const id = setInterval(() => {
          if (iter >= max) { el.textContent = final; clearInterval(id); return; }
          el.textContent = iter > max * 0.55
            ? final
            : Array.from({ length: final.length }, () =>
                CHARS[Math.floor(Math.random() * CHARS.length)]
              ).join('');
          iter++;
        }, 38);
      }, delay);
    }

    gsap.fromTo(validWords,
      { y: 100, rotateX: -70, opacity: 0, filter: 'blur(6px)' },
      {
        y: 0, rotateX: 0, opacity: 1, filter: 'blur(0px)',
        duration: 0.9, stagger: 0.06, ease: 'back.out(1.5)',
        scrollTrigger: { trigger: sectionRef, start: 'top 72%', once: true },
        onStart() {
          validWords.forEach((w, i) => scrambleWord(w, w.dataset.word, i * 60));
        }
      }
    );

    // ── Body word reveal ──────────────────────────────────────────────
    const bodySplit = new SplitText(bodyRef, { type: 'words' });
    gsap.fromTo(bodySplit.words,
      { opacity: 0, y: 18 },
      {
        opacity: 1, y: 0, duration: 0.45,
        stagger: { amount: 0.5 }, ease: 'power2.out',
        scrollTrigger: { trigger: bodyRef, start: 'top 88%', once: true }
      }
    );

    // ── SVG circuit line ──────────────────────────────────────────────
    if (lineRef) {
      gsap.set(lineRef, { drawSVG: '0%' });
      if (lineGlowRef) gsap.set(lineGlowRef, { drawSVG: '0%' });

      const lineTl = gsap.timeline({
        scrollTrigger: { trigger: cards[0], start: 'top 80%', once: true }
      });
      lineTl
        .to(lineRef, { drawSVG: '100%', duration: 2.2, ease: 'power3.inOut' }, 0)
        .to(lineGlowRef, { drawSVG: '100%', duration: 2.2, ease: 'power3.inOut' }, 0.1)
        .to([lineRef, lineGlowRef], {
          opacity: 0.3, duration: 1.5, repeat: -1, yoyo: true, ease: 'sine.inOut'
        });

      [dot1, dot2, dot3].filter(Boolean).forEach((dot, i) => {
        gsap.fromTo(dot,
          { opacity: 0, scale: 0 },
          {
            opacity: 1, scale: 1, duration: 0.4, ease: 'back.out(2)',
            delay: 0.5 + i * 0.65,
            scrollTrigger: { trigger: cards[0], start: 'top 80%', once: true }
          }
        );
        gsap.to(dot, {
          scale: 1.8, opacity: 0.4, duration: 1.3,
          repeat: -1, yoyo: true, ease: 'sine.inOut', delay: i * 0.4
        });
      });
    }

    // ── Card barrel-roll entry ────────────────────────────────────────
    const validCards = cards.filter(Boolean);
    gsap.fromTo(validCards,
      { rotateY: -90, opacity: 0, x: -60, transformPerspective: 1400, transformOrigin: 'left center' },
      {
        rotateY: 0, opacity: 1, x: 0,
        duration: 1.15, stagger: 0.16, ease: 'power4.out',
        scrollTrigger: { trigger: validCards[0], start: 'top 84%', once: true }
      }
    );

    // ── Icon spin-pop ─────────────────────────────────────────────────
    gsap.from('.step-icon', {
      scale: 0, rotation: -180, duration: 0.7, stagger: 0.18,
      ease: 'back.out(2)',
      scrollTrigger: { trigger: validCards[0], start: 'top 80%', once: true }
    });

    // ── Counter morph ─────────────────────────────────────────────────
    counterEls.filter(Boolean).forEach((el, i) => {
      const target = steps[i].stat;
      gsap.fromTo({ n: 0 }, { n: target }, {
        n: target, duration: 2.2, ease: 'expo.out',
        scrollTrigger: { trigger: validCards[0], start: 'top 84%', once: true },
        onUpdate() { el.textContent = Math.round(this.targets()[0].n) + steps[i].suffix; }
      });
    });

    // ── Ripple on card click ──────────────────────────────────────────
    validCards.forEach(card => {
      card.addEventListener('click', e => {
        const rect = card.getBoundingClientRect();
        const rip = document.createElement('div');
        Object.assign(rip.style, {
          position: 'absolute', borderRadius: '50%',
          width: '10px', height: '10px',
          left: (e.clientX - rect.left - 5) + 'px',
          top: (e.clientY - rect.top - 5) + 'px',
          background: 'rgba(197,160,89,0.5)',
          pointerEvents: 'none', zIndex: '30'
        });
        card.appendChild(rip);
        gsap.to(rip, {
          scale: 35, opacity: 0, duration: 0.85, ease: 'power2.out',
          onComplete: () => rip.remove()
        });
      });
    });

    // ── Parallax on scroll ────────────────────────────────────────────
    gsap.to(orb1, {
      y: 140, ease: 'none',
      scrollTrigger: { trigger: sectionRef, start: 'top bottom', end: 'bottom top', scrub: 2 }
    });
    gsap.to(orb2, {
      y: -80, ease: 'none',
      scrollTrigger: { trigger: sectionRef, start: 'top bottom', end: 'bottom top', scrub: 3 }
    });

    return () => {
      cancelAnimationFrame(raf);
      ScrollTrigger.getAll().forEach(t => t.kill());
      window.removeEventListener('resize', resizeCanvas);
    };
  });
</script>

<style>
  section {
    position: relative;
    overflow: hidden;
    background: var(--theme-color);
  }

  section::after {
    content: '';
    position: absolute; inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='1'/%3E%3C/svg%3E");
    opacity: 0.032;
    pointer-events: none;
    z-index: 1;
  }

  canvas {
    position: absolute; inset: 0;
    width: 100%; height: 100%;
    pointer-events: none; z-index: 0;
  }

  .scanline {
    position: absolute; left: 0; right: 0; height: 140px;
    background: linear-gradient(to bottom, transparent, rgba(197,160,89,0.022), transparent);
    pointer-events: none; z-index: 2;
  }

  .top-rule {
    position: absolute; top: 0; left: 0; right: 0; height: 1px;
    background: linear-gradient(90deg, transparent, rgba(197,160,89,0.6) 40%, rgba(197,160,89,0.6) 60%, transparent);
  }

  .orb1 {
    position: absolute; left: -12%; top: 10%;
    width: 700px; height: 700px; border-radius: 50%;
    background: radial-gradient(circle, rgba(197,160,89,0.1) 0%, transparent 65%);
    filter: blur(70px); pointer-events: none;
  }
  .orb2 {
    position: absolute; right: -10%; bottom: 5%;
    width: 550px; height: 550px; border-radius: 50%;
    background: radial-gradient(circle, rgba(197,160,89,0.07) 0%, transparent 70%);
    filter: blur(80px); pointer-events: none;
  }

  .section-label {
    display: flex; align-items: center; gap: 0.75rem;
    font-family: 'Courier New', monospace;
    font-size: 0.68rem; text-transform: uppercase;
    letter-spacing: 0.28em; color: rgba(197,160,89,0.65);
    margin-bottom: 1.75rem;
  }
  .section-label::before {
    content: ''; display: block; width: 2rem; height: 1px;
    background: rgba(197,160,89,0.5);
  }

  .headline {
    font-family: Georgia, 'Times New Roman', serif;
    font-size: clamp(2.6rem, 5.2vw, 4.6rem);
    font-weight: 400; line-height: 1.04;
    letter-spacing: -0.03em; color: #f2ede4;
    perspective: 1000px;
  }
  .word {
    display: inline-block; margin-right: 0.22em;
    transform-origin: bottom center;
    will-change: transform, opacity, filter;
  }
  .headline em { font-style: italic; color: #c5a059; }

  .body-text {
    font-size: 1.08rem; line-height: 1.85;
    color: rgba(255,255,255,0.42);
  }
  .body-text :global(strong) { color: rgba(255,255,255,0.82); font-weight: 500; }

  .cta-btn {
    display: inline-flex; align-items: center; gap: 0.75rem;
    background: linear-gradient(135deg, #c5a059 0%, #9a7430 100%);
    color: #07070a; font-weight: 700; font-size: 0.82rem;
    letter-spacing: 0.08em; text-transform: uppercase;
    padding: 1rem 2.25rem; border-radius: 999px;
    text-decoration: none; position: relative; overflow: hidden;
    transition: box-shadow 0.35s ease;
    will-change: transform;
  }
  .cta-btn:hover { box-shadow: 0 0 70px -8px rgba(197,160,89,0.55); }
  .cta-shine {
    position: absolute; inset: 0;
    background: linear-gradient(105deg, transparent 35%, rgba(255,255,255,0.22) 50%, transparent 65%);
    transform: translateX(-100%); transition: transform 0.65s ease;
  }
  .cta-btn:hover .cta-shine { transform: translateX(100%); }

  .card {
    position: relative; border-radius: 1.35rem;
    border: 1px solid rgba(255,255,255,0.06);
    background: rgba(255,255,255,0.022);
    padding: 2.5rem; overflow: hidden;
    transform-style: preserve-3d; will-change: transform;
    transition: border-color 0.4s ease, box-shadow 0.4s ease;
  }
  .card:hover {
    border-color: rgba(197,160,89,0.22);
    box-shadow: 0 0 90px -20px rgba(197,160,89,0.15);
  }
  .card::before {
    content: ''; position: absolute; inset: 0; border-radius: inherit;
    background: linear-gradient(135deg, rgba(197,160,89,0.25), transparent 55%);
    opacity: 0; transition: opacity 0.4s ease; pointer-events: none;
  }
  .card:hover::before { opacity: 1; }

  .glare {
    position: absolute; inset: 0; border-radius: inherit;
    pointer-events: none; transition: background 0.08s; z-index: 5;
  }

  .card-glow {
    position: absolute; inset: 0; border-radius: inherit;
    background: radial-gradient(ellipse at 60% -10%, rgba(197,160,89,0.1), transparent 65%);
    opacity: 0; transition: opacity 0.5s ease; pointer-events: none;
  }
  .card:hover .card-glow { opacity: 1; }

  .card-num {
    font-family: Georgia, serif;
    font-size: 5.5rem; font-weight: 300; line-height: 1;
    color: rgba(197,160,89,0.12); letter-spacing: -0.05em;
    user-select: none; transition: color 0.4s ease;
  }
  .card:hover .card-num { color: rgba(197,160,89,0.32); }

  .step-icon {
    display: flex; align-items: center; justify-content: center;
    width: 3.5rem; height: 3.5rem; border-radius: 50%;
    border: 1px solid rgba(255,255,255,0.08);
    background: rgba(0,0,0,0.35); will-change: transform;
    transition: border-color 0.4s, box-shadow 0.4s;
  }
  .card:hover .step-icon {
    border-color: rgba(197,160,89,0.4);
    box-shadow: 0 0 28px -4px rgba(197,160,89,0.3);
  }

  .card-title {
    font-family: Georgia, 'Times New Roman', serif;
    font-size: 1.45rem; font-weight: 500; color: #f2ede4;
    margin-bottom: 0.9rem; transition: color 0.3s ease; line-height: 1.25;
  }
  .card:hover .card-title { color: #c5a059; }

  .card-desc {
    font-size: 0.94rem; line-height: 1.75;
    color: rgba(255,255,255,0.38);
  }

  .counter-wrap {
    margin-top: 2rem; padding-top: 1.25rem;
    border-top: 1px solid rgba(255,255,255,0.05);
    display: flex; align-items: baseline; gap: 0.5rem;
  }
  .counter-val {
    font-family: 'Courier New', monospace;
    font-size: 1.65rem; font-weight: 400;
    color: rgba(197,160,89,0.8); letter-spacing: -0.02em; min-width: 3rem;
  }
  .counter-label {
    font-family: 'Courier New', monospace;
    font-size: 0.62rem; text-transform: uppercase;
    letter-spacing: 0.2em; color: rgba(255,255,255,0.25);
  }

  .line-dot { transform-origin: center; will-change: transform, opacity; }

  .mob-arrow { display: flex; justify-content: center; margin-top: 1.75rem; }
  .mob-arrow-ring {
    width: 2.75rem; height: 2.75rem; border-radius: 50%;
    border: 1px solid rgba(197,160,89,0.2);
    display: flex; align-items: center; justify-content: center;
    animation: abounce 2s infinite;
  }
  @keyframes abounce {
    0%,100% { transform: translateY(0); }
    50% { transform: translateY(6px); }
  }

  .bottom-strip {
    margin-top: 4rem; padding-top: 2rem;
    border-top: 1px solid rgba(255,255,255,0.05);
    display: flex; align-items: center; justify-content: space-between; flex-wrap: wrap; gap: 1rem;
  }
  .strip-text {
    font-family: 'Courier New', monospace;
    font-size: 0.63rem; letter-spacing: 0.2em; text-transform: uppercase;
  }
</style>

<section
  bind:this={sectionRef}
  class="py-32 lg:py-44"
  on:mousemove={onMagneticMove}
  role="presentation"
>
  <canvas bind:this={canvasRef}></canvas>
  <div class="top-rule"></div>
  <div class="scanline" bind:this={scanlineRef}></div>
  <div class="orb1" bind:this={orb1}></div>
  <div class="orb2" bind:this={orb2}></div>

  <div class="relative z-10 mx-auto max-w-7xl px-6">

    <!-- Header -->
    <div class="mb-24 grid grid-cols-1 items-end gap-16 lg:grid-cols-2 lg:gap-28">
      <div>
        <p class="section-label" bind:this={tagRef}>How It Works</p>
        <div class="headline">
          {#each 'Unlock Your Mortgage Potential with Marcus'.split(' ') as word, i}
            <span bind:this={headlineWords[i]} class="word" data-word={word}>{word}</span>
          {/each}
          <br />
          <em>— Experience the Difference</em>
        </div>
      </div>

      <div class="lg:pb-2">
        <div bind:this={bodyRef}>
          <p class="body-text">
            With access to over <strong>90 lenders</strong>, Marcus ensures you find the best mortgage options tailored to your needs. Enjoy fast approvals without the pressure.
          </p>
          <p class="body-text" style="margin-top:1rem;">
            Trust in a seasoned expert who prioritizes your financial goals and <strong>peace of mind</strong>.
          </p>
        </div>

        <!-- svelte-ignore event_directive_deprecated -->
        <div class="mt-10 inline-block" on:mouseleave={onMagneticLeave} role="presentation">
          <a
            bind:this={ctaRef}
            href="/contact"
            class="cta-btn"
            style="transition: transform 0.35s cubic-bezier(0.23,1,0.32,1); will-change: transform;"
          >
            <span style="position:relative;z-index:1;">Get Started Today</span>
            <svg style="position:relative;z-index:1;" width="16" height="16" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
              <path d="M5 12h14M12 5l7 7-7 7"/>
            </svg>
            <div class="cta-shine"></div>
          </a>
        </div>
      </div>
    </div>

    <!-- Steps -->
    <div class="relative">

      <!-- Desktop circuit connector -->
      <svg
        class="absolute hidden lg:block"
        style="top:88px; left:16.66%; width:66.66%; height:3px; overflow:visible; z-index:2;"
        viewBox="0 0 100 3"
        preserveAspectRatio="none"
        aria-hidden="true"
      >
        <defs>
          <filter id="svg-glow" x="-50%" y="-900%" width="200%" height="1900%">
            <feGaussianBlur stdDeviation="1.5" result="blur"/>
            <feMerge><feMergeNode in="blur"/><feMergeNode in="SourceGraphic"/></feMerge>
          </filter>
        </defs>
        <line bind:this={lineGlowRef} x1="0" y1="1.5" x2="100" y2="1.5"
          stroke="rgba(197,160,89,0.35)" stroke-width="5" stroke-linecap="round"
          filter="url(#svg-glow)"
        />
        <line bind:this={lineRef} x1="0" y1="1.5" x2="100" y2="1.5"
          stroke="rgba(197,160,89,0.7)" stroke-width="1.2" stroke-linecap="round"
          stroke-dasharray="4 7"
        />
        <circle bind:this={dot1} class="line-dot" cx="0" cy="1.5" r="3" fill="rgba(197,160,89,0.9)" />
        <circle bind:this={dot2} class="line-dot" cx="50" cy="1.5" r="3" fill="rgba(197,160,89,0.9)" />
        <circle bind:this={dot3} class="line-dot" cx="100" cy="1.5" r="3" fill="rgba(197,160,89,0.9)" />
      </svg>

      <div class="grid grid-cols-1 gap-6 lg:grid-cols-3 lg:gap-8">
        {#each steps as step, i}
          <!-- svelte-ignore a11y_no_static_element_interactions -->
          <!-- svelte-ignore event_directive_deprecated -->
          <div
            bind:this={cards[i]}
            class="card"
            on:mousemove={(e) => onCardMove(e, cards[i])}
            on:mouseleave={() => onCardLeave(cards[i])}
          >
            <div class="card-glow"></div>
            <div class="glare"></div>

            <div style="display:flex;align-items:flex-start;justify-content:space-between;margin-bottom:2.25rem;position:relative;z-index:2;">
              <span class="card-num">{step.num}</span>
              <div class="step-icon">
                <svg width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="rgba(197,160,89,0.7)" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
                  <path d={step.icon}/>
                </svg>
              </div>
            </div>

            <div style="position:relative;z-index:2;">
              <h3 class="card-title">{step.title}</h3>
              <p class="card-desc">{step.desc}</p>
              <div class="counter-wrap">
                <span class="counter-val" bind:this={counterEls[i]}>0{step.suffix}</span>
                <span class="counter-label">{step.statLabel}</span>
              </div>
            </div>

            {#if i < 2}
              <div class="mob-arrow lg:hidden">
                <div class="mob-arrow-ring">
                  <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="rgba(197,160,89,0.6)" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true">
                    <path d="M12 5v14M5 12l7 7 7-7"/>
                  </svg>
                </div>
              </div>
            {/if}
          </div>
        {/each}
      </div>
    </div>

    <!-- Bottom strip -->
    <div class="bottom-strip">
      <p class="strip-text" style="color:rgba(255,255,255,0.18);">90+ lenders · 5-day approval · 20+ years experience</p>
      <p class="strip-text" style="color:rgba(197,160,89,0.35);">Marcus Montrose · Senior Mortgage Advisor</p>
    </div>
  </div>
</section>