<script lang="ts">
  import { onMount } from 'svelte';
  import Who from "$lib/assets/stratergy/4.webp"

  let sectionRef: HTMLElement;
  let imageRef: HTMLElement;
  let contentRef: HTMLElement;
  let profileRefs: HTMLElement[] = [];

  const profiles = [
    {
      title: 'High-Income W2 Borrowers',
      desc: 'Strong earners trapped in low-liquidity mortgage structures who want more control over monthly cash flow.'
    },
    {
      title: 'Business Owners',
      desc: 'Entrepreneurs with irregular income patterns looking to deploy capital more efficiently.'
    },
    {
      title: 'Real Estate Investors',
      desc: 'Investors leveraging equity to acquire assets without disrupting existing fixed-rate positions.'
    },
    {
      title: 'Pre-Retirement Homeowners',
      desc: 'Clients seeking liquidity, payment flexibility, and long-term equity preservation strategies.'
    }
  ];

  onMount(() => {
    let ctx: any;

    const init = async () => {
      const gsap = (await import('gsap')).default;
      const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

      gsap.registerPlugin(ScrollTrigger);

      ctx = gsap.context(() => {
        gsap.from(imageRef, {
          opacity: 0,
          scale: 0.9,
          duration: 1.4,
          ease: 'power4.out',
          scrollTrigger: {
            trigger: sectionRef,
            start: 'top 75%'
          }
        });

        gsap.from(contentRef, {
          opacity: 0,
          x: 80,
          duration: 1.2,
          ease: 'power4.out',
          scrollTrigger: {
            trigger: sectionRef,
            start: 'top 75%'
          }
        });

        gsap.from(profileRefs, {
          opacity: 0,
          y: 40,
          stagger: 0.12,
          duration: 0.9,
          ease: 'power3.out',
          scrollTrigger: {
            trigger: contentRef,
            start: 'top 80%'
          }
        });

      }, sectionRef);
    };

    init();

    return () => {
      ctx?.revert();
    };
  });
</script>

<section
  bind:this={sectionRef}
  class="relative overflow-hidden bg-[#050505] py-32 lg:py-44"
>
  <!-- BG -->
  <div class="absolute inset-0 overflow-hidden">
    <div class="absolute left-[-10%] top-[5%] h-[500px] w-[500px] rounded-full bg-[#c5a059]/[0.07] blur-[140px]"></div>

    <div class="absolute bottom-[-15%] right-[-10%] h-[650px] w-[650px] rounded-full bg-white/[0.025] blur-[180px]"></div>
  </div>

  <div class="relative z-10 mx-auto max-w-7xl px-6">
    
    <div class="grid gap-20 lg:grid-cols-[1fr_0.9fr] lg:items-center">

      <!-- IMAGE SIDE -->
      <div bind:this={imageRef} class="relative">
        
        <!-- MAIN IMAGE -->
        <div class="relative overflow-hidden rounded-[38px] border border-white/10">
          <img
            src={Who}
            alt=""
            class="h-[700px] w-full object-cover"
          />

          <div class="absolute inset-0 bg-gradient-to-t from-black via-black/20 to-transparent"></div>
        </div>

        <!-- FLOATING STAT -->
        <div
          class="absolute bottom-10 left-10 max-w-[320px] rounded-[28px] border border-white/10 bg-black/50 p-7 backdrop-blur-2xl"
        >
          <p class="font-mono text-[10px] uppercase tracking-[0.3em] text-[#c5a059]">
            Strategic Positioning
          </p>

          <h3 class="mt-4 font-serif text-3xl leading-tight text-white">
            The right structure can outperform a lower rate.
          </h3>
        </div>

        <!-- SMALL FLOATING CARD -->
        <div
          class="absolute -right-8 top-10 hidden rounded-[28px] border border-white/10 bg-white/[0.04] p-6 backdrop-blur-2xl lg:block"
        >
          <p class="font-mono text-[10px] uppercase tracking-[0.25em] text-white/35">
            Capital Efficiency
          </p>

          <div class="mt-4 flex items-end gap-2">
            <span class="font-serif text-5xl text-white">
              24/7
            </span>

            <span class="pb-2 text-white/50">
              cash flow movement
            </span>
          </div>
        </div>
      </div>

      <!-- CONTENT -->
      <div bind:this={contentRef}>
        <div class="inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl">
          <div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

          <span class="font-mono text-[11px] uppercase tracking-[0.32em] text-white/50">
            Who This Strategy Is For
          </span>
        </div>

        <h2
          class="mt-8 font-serif text-[clamp(3rem,5vw,5.5rem)] leading-[0.95] tracking-[-0.04em] text-white"
        >
          Built For
          <span class="italic text-[#c5a059]">
            Financially
          </span>
          Aware Clients.
        </h2>

        <p class="mt-8 max-w-xl text-lg leading-relaxed text-white/55">
          This isn’t designed for homeowners looking for a quick fix.
          It’s built for people who want to optimize how money moves through their life.
        </p>

        <!-- PROFILES -->
        <div class="mt-14 space-y-8">
          {#each profiles as profile, i}
            <div
              bind:this={profileRefs[i]}
              class="group border-b border-white/10 pb-8"
            >
              <div class="flex items-start gap-5">
                
                <div
                  class="mt-2 h-3 w-3 rounded-full bg-[#c5a059] shadow-[0_0_20px_rgba(197,160,89,0.7)]"
                ></div>

                <div>
                  <h3
                    class="text-2xl text-white transition-colors duration-500 group-hover:text-[#c5a059]"
                  >
                    {profile.title}
                  </h3>

                  <p class="mt-4 max-w-xl text-base leading-relaxed text-white/50">
                    {profile.desc}
                  </p>
                </div>
              </div>
            </div>
          {/each}
        </div>

        <!-- BOTTOM TEXT -->
        <div class="mt-14 border-l border-[#c5a059]/30 pl-6">
          <p class="text-xl leading-relaxed text-white/70">
            The objective isn’t just borrowing money.
            <span class="text-[#c5a059]">
              It’s designing a system where capital works more intelligently.
            </span>
          </p>
        </div>
      </div>

    </div>
  </div>
</section>