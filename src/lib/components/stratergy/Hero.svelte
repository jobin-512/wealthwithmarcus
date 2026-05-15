<script lang="ts">
  import { onMount } from 'svelte';

  let sectionRef: HTMLElement;
  let spotlightRef: HTMLElement;

  let headlineWords: HTMLElement[] = [];
  let subRefs: HTMLElement[] = [];
  let statRefs: HTMLElement[] = [];

  const stats = [
    {
      number: '68%',
      label: 'of homeowners with low rates feel financially trapped'
    },
    {
      number: '30 Years',
      label: 'traditional mortgage structures maximize lender profit'
    },
    {
      number: 'Daily',
      label: 'interest calculations can completely change payoff strategy'
    }
  ];

  onMount(() => {
    let cleanup: (() => void) | undefined;

    const init = async () => {
      const gsap = (await import('gsap')).default;
      const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

      gsap.registerPlugin(ScrollTrigger);

      // Spotlight Follow
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

      const ctx = gsap.context(() => {
        // Headline
        gsap.from(headlineWords.filter(Boolean), {
          y: 140,
          opacity: 0,
          rotateX: -90,
          stagger: 0.035,
          duration: 1.2,
          ease: 'power4.out'
        });

        // Sub content
        gsap.from(subRefs.filter(Boolean), {
          opacity: 0,
          y: 50,
          stagger: 0.18,
          duration: 1,
          delay: 0.5,
          ease: 'power3.out'
        });

        // Stats
        gsap.from(statRefs.filter(Boolean), {
          opacity: 0,
          y: 80,
          stagger: 0.12,
          duration: 1.2,
          delay: 0.8,
          ease: 'power4.out'
        });

        // Floating Mesh
        gsap.to('.mesh-1', {
          y: -40,
          duration: 7,
          repeat: -1,
          yoyo: true,
          ease: 'sine.inOut'
        });

        gsap.to('.mesh-2', {
          y: 50,
          duration: 9,
          repeat: -1,
          yoyo: true,
          ease: 'sine.inOut'
        });

        // Rotating Rings
        gsap.to('.ring-1', {
          rotate: 360,
          duration: 28,
          repeat: -1,
          ease: 'none'
        });

        gsap.to('.ring-2', {
          rotate: -360,
          duration: 40,
          repeat: -1,
          ease: 'none'
        });
      }, sectionRef);

      cleanup = () => {
        ctx.revert();
        sectionRef?.removeEventListener('mousemove', moveSpotlight);
      };
    };

    init();

    return () => cleanup?.();
  });
</script>

<section
  bind:this={sectionRef}
  class="relative min-h-screen overflow-hidden bg-[#050505] pt-40"
>
  <!-- BACKGROUND -->
  <div class="noise"></div>

  <div class="mesh-1"></div>
  <div class="mesh-2"></div>

  <div class="vignette"></div>

  <!-- GRID -->
  <div class="grid-overlay"></div>

  <!-- SPOTLIGHT -->
  <div
    bind:this={spotlightRef}
    class="pointer-events-none absolute left-0 top-0 h-[520px] w-[520px] rounded-full bg-[#c5a059]/10 blur-[130px]"
  ></div>

  <!-- ORBIT -->
  <div class="ring-wrap">
    <div class="ring-1"></div>
    <div class="ring-2"></div>
  </div>

  <!-- TOP LINE -->
  <div
    class="absolute inset-x-0 top-0 h-px bg-gradient-to-r from-transparent via-white/15 to-transparent"
  ></div>

  <!-- SVG FLOW -->
  <svg
    class="pointer-events-none absolute inset-0 h-full w-full opacity-[0.08]"
    preserveAspectRatio="none"
  >
    <defs>
      <linearGradient id="strategy-line" x1="0%" y1="0%" x2="100%" y2="0%">
        <stop offset="0%" stop-color="#c5a059" stop-opacity="0" />
        <stop offset="50%" stop-color="#c5a059" stop-opacity="1" />
        <stop offset="100%" stop-color="#c5a059" stop-opacity="0" />
      </linearGradient>
    </defs>

    <path
      d="M0,180 C420,40 760,260 1080,140 S1500,80 1920,220"
      stroke="url(#strategy-line)"
      stroke-width="1"
      fill="none"
    >
      <animate
        attributeName="d"
        dur="20s"
        repeatCount="indefinite"
        values="
        M0,180 C420,40 760,260 1080,140 S1500,80 1920,220;
        M0,240 C300,320 860,20 1080,80 S1600,180 1920,120;
        M0,180 C420,40 760,260 1080,140 S1500,80 1920,220"
      />
    </path>
  </svg>

  <div class="relative z-10 mx-auto max-w-7xl px-6">
    <!-- TAG -->
    <div
      bind:this={subRefs[0]}
      class="mb-10 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-6 py-3 backdrop-blur-xl"
    >
      <div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

      <span
        class="font-mono text-[11px] uppercase tracking-[0.35em] text-white/55"
      >
        Wealth Strategy Architecture
      </span>
    </div>

    <!-- HEADLINE -->
    <div class="max-w-6xl">
      <h1
        class="font-serif text-[clamp(4rem,8vw,8rem)] leading-[0.9] tracking-[-0.06em] text-white"
        style="perspective:1000px;"
      >
        {#each 'Most homeowners are trapped in their mortgage.'.split(' ') as word, i}
          <span
            bind:this={headlineWords[i]}
            class="mr-[0.18em] inline-block origin-bottom"
            class:gold={word === 'trapped'}
          >
            {word}
          </span>
        {/each}

        <br />

        {#each 'Not because of the rate.'.split(' ') as word, i}
          <span
            bind:this={headlineWords[i + 7]}
            class="mr-[0.18em] inline-block origin-bottom"
          >
            {word}
          </span>
        {/each}

        <br />

        {#each 'Because of the structure.'.split(' ') as word, i}
          <span
            bind:this={headlineWords[i + 12]}
            class="mr-[0.18em] inline-block origin-bottom"
            class:gold={word === 'structure.'}
          >
            {word}
          </span>
        {/each}
      </h1>
    </div>

    <!-- SUBTEXT -->
    <div class="mt-14 grid gap-12 lg:grid-cols-[1.2fr_0.8fr]">
      <div class="space-y-8">
        <p
          bind:this={subRefs[1]}
          class="max-w-3xl text-xl leading-relaxed text-white/72 lg:text-2xl"
        >
          Millions of homeowners locked in ultra-low mortgage rates believe they
          can’t move, refinance, access equity, or improve cash flow
          without making a financial mistake.
        </p>

        <p
          bind:this={subRefs[2]}
          class="max-w-2xl text-base leading-relaxed text-white/42 lg:text-lg"
        >
          Traditional mortgage advice focuses on rates.
          Wealth strategy focuses on liquidity, velocity,
          access to capital, and long-term financial control.
        </p>

        <div
          bind:this={subRefs[3]}
          class="flex flex-col gap-5 pt-4 sm:flex-row"
        >
          <a
            href="/book-call"
            class="primary-btn"
          >
            Book Strategy Call

            <div class="btn-shine"></div>
          </a>

          <a
            href="/solutions"
            class="secondary-btn"
          >
            Explore Solutions
          </a>
        </div>
      </div>

      <!-- SIDE QUOTE -->
      <div
        bind:this={subRefs[4]}
        class="relative flex items-end"
      >
        <div class="quote-box">
          <div class="quote-line"></div>

          <p class="quote-text">
            “The biggest financial mistake homeowners make
            is optimizing for interest rate instead of
            optimizing for cash flow and equity access.”
          </p>
        </div>
      </div>
    </div>

    <!-- STATS -->
    <div class="mt-28 grid gap-px overflow-hidden rounded-[30px] border border-white/10 bg-white/10 lg:grid-cols-3">
      {#each stats as stat, i}
        <div
          bind:this={statRefs[i]}
          class="stat-card"
        >
          <div class="stat-glow"></div>

          <div class="relative z-10">
            <p class="stat-number">{stat.number}</p>

            <p class="stat-label">
              {stat.label}
            </p>
          </div>
        </div>
      {/each}
    </div>

    <!-- BOTTOM SPACING -->
    <div class="h-32"></div>
  </div>
</section>

<style>
  :global(body) {
    background: #050505;
  }

  .noise {
    position: absolute;
    inset: 0;

    opacity: 0.025;

    pointer-events: none;

    background-image:
      radial-gradient(rgba(255,255,255,0.35) 0.5px, transparent 0.5px);

    background-size: 4px 4px;
  }

  .grid-overlay {
    position: absolute;
    inset: 0;

    background-image:
      linear-gradient(rgba(255,255,255,0.03) 1px, transparent 1px),
      linear-gradient(90deg, rgba(255,255,255,0.03) 1px, transparent 1px);

    background-size: 120px 120px;

    mask-image: radial-gradient(circle at center, black 35%, transparent 100%);
  }

  .vignette {
    position: absolute;
    inset: 0;

    pointer-events: none;

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
    top: -220px;
    left: -200px;

    width: 720px;
    height: 720px;

    background: rgba(197,160,89,0.12);
  }

  .mesh-2 {
    right: -260px;
    bottom: -280px;

    width: 820px;
    height: 820px;

    background: rgba(255,255,255,0.04);
  }

  .ring-wrap {
    position: absolute;
    right: -180px;
    top: 120px;

    width: 720px;
    height: 720px;
  }

  .ring-1,
  .ring-2 {
    position: absolute;

    inset: 0;

    border-radius: 9999px;
  }

  .ring-1 {
    border: 1px solid rgba(197,160,89,0.12);
  }

  .ring-2 {
    inset: 80px;

    border: 1px solid rgba(255,255,255,0.06);
  }

  .gold {
    color: #c5a059;
    font-style: italic;
  }

  .primary-btn {
    position: relative;

    overflow: hidden;

    display: inline-flex;
    align-items: center;
    justify-content: center;

    min-width: 220px;

    border-radius: 9999px;

    background: #c5a059;

    padding: 18px 30px;

    color: black;

    font-size: 13px;
    font-weight: 700;
    letter-spacing: 0.16em;
    text-transform: uppercase;

    transition:
      transform 0.45s ease,
      box-shadow 0.45s ease;
  }

  .primary-btn:hover {
    transform: translateY(-4px);

    box-shadow:
      0 0 80px rgba(197,160,89,0.3);
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

    transition: transform 0.9s ease;
  }

  .primary-btn:hover .btn-shine {
    transform: translateX(120%);
  }

  .secondary-btn {
    display: inline-flex;
    align-items: center;
    justify-content: center;

    min-width: 220px;

    border-radius: 9999px;

    border: 1px solid rgba(255,255,255,0.14);

    background: rgba(255,255,255,0.03);

    padding: 18px 30px;

    color: white;

    font-size: 13px;
    font-weight: 700;
    letter-spacing: 0.16em;
    text-transform: uppercase;

    backdrop-filter: blur(20px);

    transition:
      border-color 0.45s ease,
      background 0.45s ease,
      transform 0.45s ease;
  }

  .secondary-btn:hover {
    transform: translateY(-4px);

    border-color: rgba(197,160,89,0.4);

    background: rgba(197,160,89,0.08);
  }

  .quote-box {
    position: relative;

    max-width: 420px;

    padding-left: 34px;
  }

  .quote-line {
    position: absolute;
    left: 0;
    top: 0;

    width: 1px;
    height: 100%;

    background:
      linear-gradient(
        to bottom,
        rgba(197,160,89,0),
        rgba(197,160,89,1),
        rgba(197,160,89,0)
      );
  }

  .quote-text {
    font-size: 20px;
    line-height: 1.8;
    letter-spacing: -0.01em;

    color: rgba(255,255,255,0.55);
  }

  .stat-card {
    position: relative;

    overflow: hidden;

    padding: 52px 42px;

    background: rgba(255,255,255,0.03);

    backdrop-filter: blur(20px);
  }

  .stat-glow {
    position: absolute;
    inset: 0;

    opacity: 0;

    transition: opacity 0.5s ease;

    background:
      radial-gradient(circle at top,
      rgba(197,160,89,0.14),
      transparent 70%);
  }

  .stat-card:hover .stat-glow {
    opacity: 1;
  }

  .stat-number {
    margin-bottom: 18px;

    font-family: serif;
    font-size: clamp(2.6rem,5vw,4.5rem);
    line-height: 0.9;

    color: white;
  }

  .stat-label {
    max-width: 260px;

    font-size: 15px;
    line-height: 1.8;

    color: rgba(255,255,255,0.5);
  }

  @media (max-width: 768px) {
    .ring-wrap {
      right: -300px;
      top: 200px;

      transform: scale(0.7);
    }

    .quote-box {
      padding-left: 24px;
    }

    .quote-text {
      font-size: 17px;
    }

    .stat-card {
      padding: 42px 28px;
    }
  }
</style>