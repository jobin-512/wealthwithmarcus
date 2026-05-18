<script lang="ts">
  import { onMount } from 'svelte';

  let sectionRef: HTMLElement;
  let ctaRef: HTMLElement;
  let glowRef: HTMLElement;

  onMount(() => {
    let cleanup: (() => void) | undefined;

    const init = async () => {
      const gsap = (await import('gsap')).default;
      const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

      gsap.registerPlugin(ScrollTrigger);

      // Spotlight
      const xTo = gsap.quickTo(glowRef, 'x', {
        duration: 0.8,
        ease: 'power3.out'
      });

      const yTo = gsap.quickTo(glowRef, 'y', {
        duration: 0.8,
        ease: 'power3.out'
      });

      const moveGlow = (e: MouseEvent) => {
        const rect = sectionRef.getBoundingClientRect();

        xTo(e.clientX - rect.left - 250);
        yTo(e.clientY - rect.top - 250);
      };

      sectionRef.addEventListener('mousemove', moveGlow);

      const ctx = gsap.context(() => {
        gsap.from(ctaRef, {
          opacity: 0,
          y: 100,
          scale: 0.95,
          duration: 1.4,
          ease: 'power4.out',
          scrollTrigger: {
            trigger: ctaRef,
            start: 'top 80%'
          }
        });

        gsap.from('.reveal-line', {
          scaleX: 0,
          stagger: 0.15,
          duration: 1.2,
          ease: 'power3.out',
          scrollTrigger: {
            trigger: ctaRef,
            start: 'top 82%'
          }
        });

        gsap.to('.floating-orb', {
          y: -30,
          duration: 5,
          repeat: -1,
          yoyo: true,
          ease: 'sine.inOut',
          stagger: 0.6
        });
      }, sectionRef);

      cleanup = () => {
        ctx.revert();
        sectionRef?.removeEventListener('mousemove', moveGlow);
      };
    };

    init();

    return () => cleanup?.();
  });
</script>

<section
  bind:this={sectionRef}
  class="relative overflow-hidden bg-[var(--theme-color)] py-36 lg:py-44"
>
  <!-- BACKGROUND -->
  <div class="noise"></div>

  <div class="absolute inset-0 overflow-hidden">
    <div class="floating-orb orb-1"></div>
    <div class="floating-orb orb-2"></div>
    <div class="floating-orb orb-3"></div>
  </div>

  <div class="vignette"></div>

  <!-- SPOTLIGHT -->
  <div
    bind:this={glowRef}
    class="pointer-events-none absolute left-0 top-0 h-[520px] w-[520px] rounded-full bg-[#c5a059]/10 blur-[140px]"
  ></div>

  <!-- TOP BORDER -->
  <div
    class="absolute inset-x-0 top-0 h-px bg-gradient-to-r from-transparent via-white/10 to-transparent"
  ></div>

  <!-- SVG -->
  <svg
    class="pointer-events-none absolute inset-0 h-full w-full opacity-[0.08]"
    preserveAspectRatio="none"
  >
    <defs>
      <linearGradient id="final-gold" x1="0%" y1="0%" x2="100%" y2="0%">
        <stop offset="0%" stop-color="#c5a059" stop-opacity="0" />
        <stop offset="50%" stop-color="#c5a059" stop-opacity="1" />
        <stop offset="100%" stop-color="#c5a059" stop-opacity="0" />
      </linearGradient>
    </defs>

    <path
      d="M0,160 C400,40 760,240 1100,120 S1500,100 1920,220"
      stroke="url(#final-gold)"
      stroke-width="1"
      fill="none"
    >
      <animate
        attributeName="d"
        dur="18s"
        repeatCount="indefinite"
        values="
        M0,160 C400,40 760,240 1100,120 S1500,100 1920,220;
        M0,210 C300,280 860,20 1100,90 S1600,160 1920,140;
        M0,160 C400,40 760,240 1100,120 S1500,100 1920,220"
      />
    </path>
  </svg>

  <div class="relative z-10 mx-auto max-w-7xl px-6">
    <div
      bind:this={ctaRef}
      class="relative overflow-hidden rounded-[42px] border border-white/10 bg-white/[0.04] px-8 py-16 backdrop-blur-2xl lg:px-20 lg:py-24"
    >
      <!-- INNER GLOW -->
      <div class="inner-glow"></div>

      <!-- SHINE -->
      <div class="shine"></div>

      <div class="relative z-10 mx-auto max-w-5xl text-center">
        <!-- TAG -->
        <div
          class="mb-8 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3"
        >
          <div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

          <span
            class="font-mono text-[11px] uppercase tracking-[0.35em] text-white/55"
          >
            Strategy Before Products
          </span>
        </div>

        <!-- HEADING -->
        <h2
          class="mx-auto max-w-5xl font-serif text-[clamp(3rem,6vw,6.5rem)] leading-[0.92] tracking-[-0.05em] text-white"
        >
          Most homeowners don’t need a
          <span class="italic text-[#c5a059]">new mortgage.</span>

          <br />

          They need a
          <span class="italic text-[#c5a059]">better strategy.</span>
        </h2>

        <!-- LINES -->
        <div class="mx-auto mt-14 flex max-w-4xl flex-col gap-5">
          <div class="overflow-hidden rounded-full bg-white/5">
            <div class="reveal-line h-[1px] w-full bg-[#c5a059]"></div>
          </div>

          <div class="overflow-hidden rounded-full bg-white/5">
            <div
              class="reveal-line h-[1px] w-full bg-gradient-to-r from-transparent via-white/40 to-transparent"
            ></div>
          </div>
        </div>

        <!-- COPY -->
        <div class="mx-auto mt-14 max-w-3xl space-y-7">
          <p
            class="text-xl leading-relaxed text-white/72 lg:text-2xl"
          >
            Whether you want to eliminate debt faster, access equity without
            refinancing, or optimize long-term cash flow —
            <span class="text-white">
              we structure lending around outcomes, not commissions.
            </span>
          </p>

          <p
            class="text-base leading-relaxed text-white/42 lg:text-lg"
          >
            This is not traditional mortgage planning.
            This is strategic capital positioning designed for modern homeowners,
            investors, and high-income borrowers.
          </p>
        </div>

        <!-- BUTTONS -->
        <div
          class="mt-16 flex flex-col items-center justify-center gap-5 sm:flex-row"
        >
          <a
            href="/book-call"
            class="primary-btn"
          >
            <span class="relative z-10">Book Strategy Call</span>

            <div class="btn-shine"></div>
          </a>

          <a
            href="/strategy"
            class="secondary-btn"
          >
            Explore The Strategy
          </a>
        </div>

        <!-- STATS -->
        <div
          class="mt-20 grid grid-cols-1 gap-10 border-t border-white/10 pt-12 text-center sm:grid-cols-3"
        >
          <div>
            <p class="stat-number">5–7 Years</p>

            <p class="stat-label">
              Potential mortgage payoff strategy timeline
            </p>
          </div>

          <div>
            <p class="stat-number">$500K–$4M</p>

            <p class="stat-label">
              Proprietary reverse mortgage solutions
            </p>
          </div>

          <div>
            <p class="stat-number">Daily Interest</p>

            <p class="stat-label">
              Optimization strategies built around cash flow velocity
            </p>
          </div>
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

    opacity: 0.025;

    pointer-events: none;

    background-image:
      radial-gradient(rgba(255,255,255,0.35) 0.5px, transparent 0.5px);

    background-size: 4px 4px;
  }

  .vignette {
    position: absolute;
    inset: 0;

    pointer-events: none;

    background:
      radial-gradient(circle at center,
      transparent 20%,
      rgba(0,0,0,0.78) 100%);
  }

  .floating-orb {
    position: absolute;

    border-radius: 9999px;

    filter: blur(120px);
  }

  .orb-1 {
    left: -180px;
    top: -120px;

    width: 500px;
    height: 500px;

    background: rgba(197,160,89,0.12);
  }

  .orb-2 {
    right: -220px;
    top: 40%;

    width: 560px;
    height: 560px;

    background: rgba(255,255,255,0.04);
  }

  .orb-3 {
    bottom: -240px;
    left: 40%;

    width: 520px;
    height: 520px;

    background: rgba(197,160,89,0.08);
  }

  .inner-glow {
    position: absolute;
    inset: 0;

    background:
      radial-gradient(circle at top,
      rgba(197,160,89,0.16),
      transparent 65%);
  }

  .shine {
    position: absolute;
    inset: 0;

    background:
      linear-gradient(
        115deg,
        transparent 40%,
        rgba(255,255,255,0.04) 50%,
        transparent 60%
      );

    transform: translateX(-100%) rotate(12deg);

    animation: shineMove 9s linear infinite;
  }

  .primary-btn {
    position: relative;

    overflow: hidden;

    min-width: 240px;

    border-radius: 9999px;

    background: #c5a059;

    padding: 18px 32px;

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
        rgba(255,255,255,0.4) 50%,
        transparent 65%
      );

    transform: translateX(-120%);

    transition: transform 0.9s ease;
  }

  .primary-btn:hover .btn-shine {
    transform: translateX(120%);
  }

  .secondary-btn {
    min-width: 240px;

    border-radius: 9999px;

    border: 1px solid rgba(255,255,255,0.14);

    background: rgba(255,255,255,0.03);

    padding: 18px 32px;

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

  .stat-number {
    margin-bottom: 10px;

    font-family: serif;
    font-size: clamp(2rem,4vw,3rem);
    line-height: 1;

    color: white;
  }

  .stat-label {
    max-width: 240px;
    margin: 0 auto;

    font-size: 13px;
    line-height: 1.8;
    letter-spacing: 0.04em;

    color: rgba(255,255,255,0.45);
  }

  @keyframes shineMove {
    from {
      transform: translateX(-100%) rotate(12deg);
    }

    to {
      transform: translateX(100%) rotate(12deg);
    }
  }
</style>