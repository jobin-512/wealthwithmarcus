<script lang="ts">
  import { onMount } from 'svelte';

  let sectionRef: HTMLElement;
  let pinRef: HTMLElement;
  let compareRefs: HTMLElement[] = $state([]);

  const comparisons = [
    {
      label: 'Traditional Mortgage',
      old: '30 Years',
      new: '5–7 Years',
      desc: 'Typical payoff timeline vs accelerated cash-flow strategy.'
    },
    {
      label: 'Interest Paid',
      old: '$480,000+',
      new: 'Reduced Dramatically',
      desc: 'Less idle cash means less long-term interest drag.'
    },
    {
      label: 'Monthly Structure',
      old: 'Fixed Payment Cycle',
      new: 'Dynamic Equity Movement',
      desc: 'Money constantly offsets principal instead of sitting dormant.'
    }
  ];

  onMount(() => {
    let ctx: any;

    const init = async () => {
      const gsap = (await import('gsap')).default;
      const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

      gsap.registerPlugin(ScrollTrigger);

      ctx = gsap.context(() => {
        gsap.from('.compare-heading', {
          opacity: 0,
          y: 80,
          duration: 1.2,
          ease: 'power4.out',
          scrollTrigger: {
            trigger: sectionRef,
            start: 'top 75%'
          }
        });

        gsap.from(compareRefs, {
          stagger: 0.15,
          duration: 1,
          ease: 'power3.out',
          scrollTrigger: {
            trigger: sectionRef,
            start: 'top 75%'
          }
        });

        gsap.to('.floating-orb', {
          y: -40,
          duration: 6,
          repeat: -1,
          yoyo: true,
          ease: 'sine.inOut'
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
  class="relative overflow-hidden bg-[#070707] py-32 lg:py-44"
>
  <!-- BG -->
  <div class="absolute inset-0 overflow-hidden">
    <div class="floating-orb absolute left-[10%] top-[15%] h-[340px] w-[340px] rounded-full bg-[#c5a059]/[0.06] blur-[120px]"></div>

    <div class="absolute bottom-[5%] right-[5%] h-[420px] w-[420px] rounded-full bg-white/[0.03] blur-[140px]"></div>
  </div>

  <!-- GRID -->
  <div class="relative z-10 mx-auto max-w-7xl px-6">
    
    <!-- HEADER -->
    <div class="compare-heading mx-auto max-w-4xl text-center">
      <div class="inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl">
        <div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

        <span class="font-mono text-[11px] uppercase tracking-[0.32em] text-white/50">
          Mortgage Math Reimagined
        </span>
      </div>

      <h2
        class="mt-8 font-serif text-[clamp(3rem,6vw,6rem)] leading-[0.95] tracking-[-0.04em] text-white"
      >
        Same Income.
        <br />
        <span class="italic text-[#c5a059]">
          Different Outcome.
        </span>
      </h2>

      <p class="mx-auto mt-8 max-w-3xl text-xl leading-relaxed text-white/55">
        Most homeowners are not trapped by their interest rate.
        They’re trapped by the way cash flow moves through their financial system.
      </p>
    </div>

    <!-- COMPARISON -->
    <div
      bind:this={pinRef}
      class="mt-24 space-y-10"
    >
      {#each comparisons as item, i}
        <div
          bind:this={compareRefs[i]}
          class="group relative overflow-hidden rounded-[36px] border border-white/10 bg-white/[0.03] p-10 backdrop-blur-2xl transition-all duration-500 hover:border-[#c5a059]/30"
        >
          <!-- SHINE -->
          <div class="compare-shine"></div>

          <div class="relative z-10 grid gap-10 lg:grid-cols-[0.8fr_1fr_1fr] lg:items-center">
            
            <!-- LABEL -->
            <div>
              <p class="font-mono text-[11px] uppercase tracking-[0.28em] text-white/35">
                {item.label}
              </p>

              <p class="mt-5 text-lg leading-relaxed text-white/55">
                {item.desc}
              </p>
            </div>

            <!-- OLD -->
            <div class="relative overflow-hidden rounded-[28px] border border-red-500/10 bg-red-500/[0.03] p-8">
              <div class="absolute right-5 top-5 rounded-full border border-red-500/20 bg-red-500/10 px-3 py-1">
                <span class="text-[10px] uppercase tracking-[0.2em] text-red-300">
                  Old System
                </span>
              </div>

              <p class="font-serif text-5xl text-white/30 line-through">
                {item.old}
              </p>
            </div>

            <!-- NEW -->
            <div class="relative overflow-hidden rounded-[28px] border border-[#c5a059]/20 bg-[#c5a059]/[0.05] p-8">
              <div class="absolute right-5 top-5 rounded-full border border-[#c5a059]/30 bg-[#c5a059]/10 px-3 py-1">
                <span class="text-[10px] uppercase tracking-[0.2em] text-[#e7c98d]">
                  Strategy System
                </span>
              </div>

              <p class="font-serif text-5xl text-white">
                {item.new}
              </p>
            </div>
          </div>
        </div>
      {/each}
    </div>

    <!-- QUOTE -->
    <div class="mt-28 text-center">
      <p
        class="mx-auto max-w-5xl font-serif text-[clamp(2rem,4vw,4rem)] leading-[1.15] tracking-[-0.03em] text-white"
      >
        “The goal isn’t just to get a lower rate.
        <span class="text-[#c5a059] italic">
          The goal is to reduce how long interest controls your life.”
        </span>
      </p>
    </div>
  </div>
</section>

<style>
  .compare-shine {
    position: absolute;
    inset: 0;

    background:
      linear-gradient(
        115deg,
        transparent 35%,
        rgba(255,255,255,0.04) 50%,
        transparent 65%
      );

    transform: translateX(-120%) rotate(12deg);

    animation: shineMove 9s linear infinite;

    pointer-events: none;
  }

  @keyframes shineMove {
    from {
      transform: translateX(-120%) rotate(12deg);
    }

    to {
      transform: translateX(120%) rotate(12deg);
    }
  }
</style>