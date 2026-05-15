<script lang="ts">
  import { onMount } from 'svelte';

  let sectionRef: HTMLElement;
  let headlineRef: HTMLElement;
  let ctaRef: HTMLElement;
  let statsRef: HTMLElement[] = [];

  const stats = [
    {
      value: '$500K–$4M',
      label: 'Proprietary Reverse Solutions'
    },
    {
      value: '5–7 Years',
      label: 'Potential Mortgage Elimination Strategy'
    },
    {
      value: '24/7',
      label: 'Capital Movement Optimization'
    }
  ];

  onMount(() => {
    let ctx: any;

    const init = async () => {
      const gsap = (await import('gsap')).default;
      const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

      gsap.registerPlugin(ScrollTrigger);

      ctx = gsap.context(() => {

        gsap.from(headlineRef, {
          opacity: 0,
          y: 80,
          duration: 1.3,
          ease: 'power4.out',
          scrollTrigger: {
            trigger: sectionRef,
            start: 'top 75%'
          }
        });

        gsap.from(statsRef, {
          stagger: 0.15,
          duration: 1,
          ease: 'power3.out',
          scrollTrigger: {
            trigger: ctaRef,
            start: 'top 82%'
          }
        });

        gsap.from('.cta-btn', {
          stagger: 0.12,
          duration: 1,
          ease: 'power3.out',
          scrollTrigger: {
            trigger: ctaRef,
            start: 'top 82%'
          }
        });

        gsap.to('.orb-float', {
          y: -40,
          duration: 7,
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
  class="relative overflow-hidden bg-[#050505] py-36 lg:py-52"
>
  <!-- BACKGROUND -->
  <div class="absolute inset-0 overflow-hidden">
    
    <div class="orb-float absolute left-[5%] top-[10%] h-[500px] w-[500px] rounded-full bg-[#c5a059]/[0.08] blur-[150px]"></div>

    <div class="absolute bottom-[-15%] right-[-10%] h-[700px] w-[700px] rounded-full bg-white/[0.03] blur-[180px]"></div>

    <!-- GRID -->
    <div
      class="absolute inset-0 opacity-[0.04]"
      style="
        background-image:
        linear-gradient(rgba(255,255,255,0.08) 1px, transparent 1px),
        linear-gradient(90deg, rgba(255,255,255,0.08) 1px, transparent 1px);
        background-size: 120px 120px;
      "
    ></div>
  </div>

  <!-- TOP LINE -->
  <div
    class="absolute inset-x-0 top-0 h-px bg-gradient-to-r from-transparent via-white/15 to-transparent"
  ></div>

  <div class="relative z-10 mx-auto max-w-7xl px-6">

    <!-- CENTER -->
    <div
      bind:this={headlineRef}
      class="mx-auto max-w-6xl text-center"
    >
      <!-- TAG -->
      <div
        class="inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-6 py-3 backdrop-blur-xl"
      >
        <div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

        <span class="font-mono text-[11px] uppercase tracking-[0.32em] text-white/45">
          Build A Smarter Financial Structure
        </span>
      </div>

      <!-- HEADING -->
      <h2
        class="mt-10 font-serif text-[clamp(3.5rem,7vw,8rem)] leading-[0.9] tracking-[-0.05em] text-white"
      >
        Stop Letting
        <br />

        <span class="italic text-[#c5a059]">
          Interest Control
        </span>

        Your Future.
      </h2>

      <!-- TEXT -->
      <p
        class="mx-auto mt-10 max-w-3xl text-xl leading-relaxed text-white/55 lg:text-2xl"
      >
        Most homeowners never see the financial structures available to them.

        We help clients unlock liquidity, optimize cash flow,
        and build long-term leverage strategies around real equity.
      </p>
    </div>

    <!-- STATS -->
    <div
      bind:this={ctaRef}
      class="mt-24 grid gap-6 lg:grid-cols-3"
    >
      {#each stats as stat, i}
        <div
          bind:this={statsRef[i]}
          class="group relative overflow-hidden rounded-[34px] border border-white/10 bg-white/[0.03] p-10 backdrop-blur-2xl transition-all duration-500 hover:border-[#c5a059]/30"
        >
          <!-- SHINE -->
          <div class="shine"></div>

          <div class="relative z-10">
            <p
              class="font-serif text-5xl leading-none tracking-[-0.03em] text-white"
            >
              {stat.value}
            </p>

            <p
              class="mt-5 text-base leading-relaxed text-white/50"
            >
              {stat.label}
            </p>
          </div>
        </div>
      {/each}
    </div>

    <!-- CTA -->
    <div class="mt-20 flex flex-col items-center justify-center gap-5 sm:flex-row">
      
      <a
        href="/book-call"
        class="cta-btn group relative overflow-hidden rounded-full bg-[#c5a059] px-10 py-5"
      >
        <span
          class="relative z-10 flex items-center gap-3 text-sm font-semibold uppercase tracking-[0.2em] text-black"
        >
          Book Strategy Call

          <svg
            class="h-4 w-4 transition-transform duration-500 group-hover:translate-x-1"
            fill="none"
            viewBox="0 0 24 24"
            stroke="currentColor"
            stroke-width="2.5"
          >
            <path
              stroke-linecap="round"
              stroke-linejoin="round"
              d="M17 8l4 4m0 0l-4 4m4-4H3"
            />
          </svg>
        </span>

        <div class="btn-shine"></div>
      </a>

      <a
        href="/solutions"
        class="cta-btn rounded-full border border-white/10 bg-white/[0.03] px-10 py-5 text-sm font-medium uppercase tracking-[0.2em] text-white transition-all duration-500 hover:border-white/20 hover:bg-white/[0.06]"
      >
        Explore Solutions
      </a>
    </div>

    <!-- BOTTOM -->
    <div class="mt-24 text-center">
      <p
        class="font-serif text-[clamp(1.8rem,3vw,3rem)] leading-[1.2] tracking-[-0.03em] text-white/85"
      >
        “The wealthy don’t just earn differently.
        <span class="text-[#c5a059] italic">
          They structure differently.”
        </span>
      </p>
    </div>
  </div>
</section>

<style>
  .shine {
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

  .btn-shine {
    position: absolute;
    inset: 0;

    background:
      linear-gradient(
        115deg,
        transparent 35%,
        rgba(255,255,255,0.45) 50%,
        transparent 65%
      );

    transform: translateX(-120%);

    transition: transform 0.8s ease;
  }

  .group:hover .btn-shine {
    transform: translateX(120%);
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