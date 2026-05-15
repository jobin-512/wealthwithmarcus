<script lang="ts">
  import { onMount } from 'svelte';
  import Hero from '$lib/assets/helloc/1.webp';

  let sectionRef: HTMLElement;
  let headlineRef: HTMLElement;
  let visualRef: HTMLElement;
  let floatingCardRefs: HTMLElement[] = [];
  let spotlightRef: HTMLElement;

  const metrics = [
    {
      value: '5–7',
      label: 'Potential Years To Mortgage Freedom'
    },
    {
      value: '24/7',
      label: 'Cash Flow Optimization'
    },
    {
      value: '$0',
      label: 'Required Refinancing'
    }
  ];

  onMount(() => {
    let ctx: any;

    const init = async () => {
      const gsap = (await import('gsap')).default;
      const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

      gsap.registerPlugin(ScrollTrigger);

      // Spotlight
      const xTo = gsap.quickTo(spotlightRef, 'x', {
        duration: 0.9,
        ease: 'power3.out'
      });

      const yTo = gsap.quickTo(spotlightRef, 'y', {
        duration: 0.9,
        ease: 'power3.out'
      });

      const moveSpotlight = (e: MouseEvent) => {
        const rect = sectionRef.getBoundingClientRect();

        xTo(e.clientX - rect.left - 250);
        yTo(e.clientY - rect.top - 250);
      };

      sectionRef.addEventListener('mousemove', moveSpotlight);

      ctx = gsap.context(() => {

        gsap.from('.headline-word', {
          y: 140,
          opacity: 0,
          rotateX: -90,
          stagger: 0.05,
          duration: 1.3,
          ease: 'power4.out'
        });

        gsap.from('.hero-text', {
          opacity: 0,
          y: 40,
          stagger: 0.12,
          duration: 1,
          delay: 0.5,
          ease: 'power3.out'
        });

        gsap.from(visualRef, {
          opacity: 0,
          scale: 0.92,
          x: 80,
          duration: 1.5,
          ease: 'power4.out'
        });

        gsap.from(floatingCardRefs, {
          opacity: 0,
          y: 50,
          stagger: 0.12,
          duration: 1,
          delay: 0.7,
          ease: 'power3.out'
        });

        gsap.to('.float-card', {
          y: -18,
          duration: 4,
          repeat: -1,
          yoyo: true,
          ease: 'sine.inOut',
          stagger: 0.2
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
  class="relative overflow-hidden bg-[#050505] pb-24 pt-40 lg:min-h-screen lg:pb-32 lg:pt-44"
>
  <!-- BACKGROUND -->
  <div class="noise"></div>

  <div class="mesh-1"></div>
  <div class="mesh-2"></div>

  <div class="vignette"></div>

  <!-- SPOTLIGHT -->
  <div
    bind:this={spotlightRef}
    class="pointer-events-none absolute left-0 top-0 h-[500px] w-[500px] rounded-full bg-[#c5a059]/10 blur-[120px]"
  ></div>

  <!-- GRID -->
  <div class="relative z-10 mx-auto grid max-w-7xl gap-20 px-6 lg:grid-cols-[1fr_0.95fr] lg:items-center">

    <!-- LEFT -->
    <div>
      <!-- TAG -->
      <div
        class="hero-text inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl"
      >
        <div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

        <span class="font-mono text-[11px] uppercase tracking-[0.32em] text-white/50">
          First Lien HELOC Strategy
        </span>
      </div>

      <!-- HEADLINE -->
      <h1
        bind:this={headlineRef}
        class="mt-10 font-serif text-[clamp(4rem,8vw,8.5rem)] leading-[0.9] tracking-[-0.06em] text-white"
        style="perspective:1000px;"
      >
        {#each [
          'Pay',
          'Off',
          'Your',
          'Mortgage',
          'In',
          '5–7',
          'Years'
        ] as word}
          <span class="headline-word mr-[0.18em] inline-block origin-bottom">
            {word}
          </span>
        {/each}

        <br />

        <span
          class="headline-word inline-block italic text-[#c5a059]"
        >
          Without Refinancing.
        </span>
      </h1>

      <!-- TEXT -->
      <p
        class="hero-text mt-10 max-w-2xl text-xl leading-relaxed text-white/60 lg:text-2xl"
      >
        Most homeowners are trapped —
        not because of their interest rate,
        but because of how their money moves.

        This strategy restructures cash flow
        to reduce interest drag and accelerate principal payoff.
      </p>

      <!-- CTA -->
      <div class="hero-text mt-12 flex flex-col gap-5 sm:flex-row">
        <a
          href="/book-call"
          class="group relative overflow-hidden rounded-full bg-[#c5a059] px-9 py-5"
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
          href="/strategy"
          class="rounded-full border border-white/10 bg-white/[0.03] px-9 py-5 text-sm font-medium uppercase tracking-[0.2em] text-white transition-all duration-500 hover:border-white/20 hover:bg-white/[0.05]"
        >
          Learn The Strategy
        </a>
      </div>

      <!-- METRICS -->
      <div class="mt-16 grid gap-5 sm:grid-cols-3">
        {#each metrics as metric, i}
          <div
            bind:this={floatingCardRefs[i]}
            class="float-card rounded-[28px] border border-white/10 bg-white/[0.03] p-6 backdrop-blur-2xl"
          >
            <p
              class="font-serif text-5xl leading-none tracking-[-0.04em] text-white"
            >
              {metric.value}
            </p>

            <p class="mt-4 text-sm leading-relaxed text-white/45">
              {metric.label}
            </p>
          </div>
        {/each}
      </div>
    </div>

    <!-- RIGHT -->
    <div
      bind:this={visualRef}
      class="relative"
    >
      <!-- IMAGE -->
      <div class="relative overflow-hidden rounded-[40px] border border-white/10">
        <img
          src={Hero}
          alt=""
          class="h-[760px] w-full object-cover"
        />

        <div class="absolute inset-0 bg-gradient-to-t from-black via-black/20 to-transparent"></div>
      </div>

      <!-- FLOATING OVERLAY -->
      <div
        class="absolute bottom-10 left-10 max-w-[340px] rounded-[28px] border border-white/10 bg-black/50 p-7 backdrop-blur-2xl"
      >
        <p
          class="font-mono text-[10px] uppercase tracking-[0.28em] text-[#c5a059]"
        >
          The Hidden Problem
        </p>

        <h3
          class="mt-4 font-serif text-3xl leading-tight text-white"
        >
          Most monthly mortgage payments barely touch principal.
        </h3>

        <p
          class="mt-5 text-base leading-relaxed text-white/55"
        >
          The system was designed for long-term interest extraction —
          not accelerated ownership.
        </p>
      </div>

      <!-- SMALL FLOAT -->
      <div
        class="absolute -left-8 top-10 hidden rounded-[26px] border border-white/10 bg-white/[0.04] p-6 backdrop-blur-2xl lg:block"
      >
        <p
          class="font-mono text-[10px] uppercase tracking-[0.25em] text-white/35"
        >
          Strategy Advantage
        </p>

        <div class="mt-4">
          <p class="font-serif text-5xl text-[#c5a059]">
            Daily
          </p>

          <p class="mt-2 text-sm text-white/45">
            interest reduction potential
          </p>
        </div>
      </div>
    </div>
  </div>
</section>

<style>
  :global(body) {
    background: #050505;
  }

  .noise {
    position: absolute;
    inset: 0;
    opacity: 0.03;
    pointer-events: none;

    background-image:
      radial-gradient(rgba(255,255,255,0.4) 0.5px, transparent 0.5px);

    background-size: 4px 4px;
  }

  .vignette {
    position: absolute;
    inset: 0;

    background:
      radial-gradient(circle at center,
      transparent 20%,
      rgba(0,0,0,0.82) 100%);
  }

  .mesh-1,
  .mesh-2 {
    position: absolute;
    border-radius: 9999px;
    filter: blur(120px);
  }

  .mesh-1 {
    left: -200px;
    top: -200px;

    width: 700px;
    height: 700px;

    background: rgba(197,160,89,0.1);
  }

  .mesh-2 {
    right: -250px;
    bottom: -250px;

    width: 800px;
    height: 800px;

    background: rgba(197,160,89,0.08);
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
</style>