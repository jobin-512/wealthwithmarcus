<script lang="ts">
  // @ts-nocheck

  import { onMount } from 'svelte';

  let sectionRef: HTMLElement;
  let formRef: HTMLElement;
  let spotlightRef: HTMLElement;

  let headlineWords: HTMLElement[] = $state([]) ;
  let selected: string | null = $state(null);
  let progress = $state(0);

  const options = [
    { id: 'lower', label: 'Lower my mortgage payment' },
    { id: 'cashout', label: 'Cash out for debt, home, or investments' },
    { id: 'eliminate', label: 'Eliminate mortgage payments (55+)' },
    { id: 'second', label: '2nd mtg, no payments, keep 1st in place' },
    { id: 'buy', label: 'Buy a home or investment property' },
    { id: 'funding', label: 'Get personal or business-purpose funding' }
  ];

  function selectOption(id: string) {
    selected = id;
    progress = 100;
  }

  onMount(async () => {
    const gsap = (await import('gsap')).default;
    const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

    gsap.registerPlugin(ScrollTrigger);

    // SPOTLIGHT
    const xTo = gsap.quickTo(spotlightRef, 'x', {
      duration: 0.8,
      ease: 'power3.out'
    });

    const yTo = gsap.quickTo(spotlightRef, 'y', {
      duration: 0.8,
      ease: 'power3.out'
    });

    const moveSpotlight = (e: MouseEvent) => {
      const rect = sectionRef.getBoundingClientRect();

      xTo(e.clientX - rect.left - 250);
      yTo(e.clientY - rect.top - 250);
    };

    sectionRef.addEventListener('mousemove', moveSpotlight);

    const ctx = gsap.context(() => {
      // SECTION
      gsap.from(sectionRef, {
        duration: 1.2,
        ease: 'power3.out'
      });

      // HEADLINE
      gsap.from(headlineWords.filter(Boolean), {
        y: 120,
        rotateX: -70,
        stagger: 0.05,
        duration: 1,
        ease: 'power4.out',
        transformOrigin: '50% 100%',
        scrollTrigger: {
          trigger: sectionRef,
          start: 'top 80%'
        }
      });

      // CARD
      gsap.from(formRef, {
        y: 80,
        scale: 0.96,
        duration: 1.1,
        ease: 'power4.out',
        scrollTrigger: {
          trigger: formRef,
          start: 'top 85%'
        }
      });

      // LOAD ALL OPTIONS TOGETHER
      gsap.from('.quiz-option', {
        scale: 0.96,
        duration: 0.8,
        stagger: 0.05,
        ease: 'power3.out',
        delay: 0.2,
        scrollTrigger: {
          trigger: formRef,
          start: 'top 85%'
        }
      });

      // MAGNETIC EFFECT
      formRef.addEventListener('mousemove', (e: MouseEvent) => {
        const rect = formRef.getBoundingClientRect();

        const x = e.clientX - rect.left;
        const y = e.clientY - rect.top;

        gsap.to(formRef, {
          rotateY: gsap.utils.mapRange(0, rect.width, -5, 5, x),
          rotateX: gsap.utils.mapRange(0, rect.height, 5, -5, y),
          transformPerspective: 1200,
          duration: 0.6,
          ease: 'power3.out'
        });
      });

      formRef.addEventListener('mouseleave', () => {
        gsap.to(formRef, {
          rotateX: 0,
          rotateY: 0,
          duration: 1,
          ease: 'power3.out'
        });
      });

      // PARALLAX
      gsap.to('.mesh1', {
        yPercent: -10,
        ease: 'none',
        scrollTrigger: {
          trigger: sectionRef,
          scrub: true
        }
      });

      gsap.to('.mesh2', {
        yPercent: 10,
        ease: 'none',
        scrollTrigger: {
          trigger: sectionRef,
          scrub: true
        }
      });
    }, sectionRef);

    return () => {
      ctx.revert();
      sectionRef.removeEventListener('mousemove', moveSpotlight);
    };
  });
</script>

<section
  bind:this={sectionRef}
  class="relative overflow-hidden bg-[#050505] py-32 lg:py-44"
>
  <!-- BACKGROUND -->
  <div class="noise"></div>

  <div class="mesh1"></div>
  <div class="mesh2"></div>

  <div class="vignette"></div>

  <div class="particles"></div>

  <!-- SPOTLIGHT -->
  <div
    bind:this={spotlightRef}
    class="pointer-events-none absolute left-0 top-0 z-[1] h-[500px] w-[500px] rounded-full bg-[#c5a059]/10 blur-[120px]"
  ></div>

  <!-- TOP BORDER -->
  <div
    class="absolute inset-x-0 top-0 h-px bg-gradient-to-r from-transparent via-white/15 to-transparent"
  ></div>

  <!-- SVG LINES -->
  <svg
    class="pointer-events-none absolute inset-0 h-full w-full opacity-[0.16]"
    preserveAspectRatio="none"
  >
    <defs>
      <linearGradient id="gold-line" x1="0%" y1="0%" x2="100%" y2="0%">
        <stop offset="0%" stop-color="#c5a059" stop-opacity="0" />
        <stop offset="50%" stop-color="#c5a059" stop-opacity="1" />
        <stop offset="100%" stop-color="#c5a059" stop-opacity="0" />
      </linearGradient>
    </defs>

    <path
      d="M0,150 C400,50 700,250 960,150 S1400,100 1920,200"
      stroke="url(#gold-line)"
      stroke-width="1"
      fill="none"
    >
      <animate
        attributeName="d"
        dur="18s"
        repeatCount="indefinite"
        values="
        M0,150 C400,50 700,250 960,150 S1400,100 1920,200;
        M0,200 C300,250 800,50 960,100 S1300,150 1920,100;
        M0,150 C400,50 700,250 960,150 S1400,100 1920,200"
      />
    </path>
  </svg>

  <div class="relative z-10 mx-auto max-w-7xl px-6">
    <div class="grid grid-cols-1 items-center gap-20 lg:grid-cols-2">
      <!-- LEFT -->
      <div class="relative">
        <div
          class="mb-7 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl"
        >
          <div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

          <span
            class="font-mono text-[11px] uppercase tracking-[0.32em] text-white/55"
          >
            Explore Your Options. Get Funded in 5 Days.
          </span>
        </div>

        <h2
          class="max-w-[750px] font-serif text-[clamp(3rem,6vw,6.5rem)] leading-[0.95] tracking-[-0.04em] text-white"
          style="perspective:1000px;"
        >
          {#each 'Ready to Take the Next Step?'.split(' ') as word, i}
            <span
              bind:this={headlineWords[i]}
              class="mr-[0.22em] inline-block origin-bottom"
            >
              {word}
            </span>
          {/each}
        </h2>

        <p
          class="mt-10 max-w-xl text-lg leading-relaxed text-white/55 lg:text-xl"
        >
          In just 60 seconds, explore refinancing, equity access, or business
          funding — tailored strategies and proven solutions that put you back
          in control.
        </p>

        <div class="mt-14 flex flex-wrap gap-8">
          <div
            class="rounded-2xl border border-white/10 bg-white/[0.03] px-8 py-6 backdrop-blur-xl"
          >
            <p class="font-serif text-5xl text-[#c5a059]">60s</p>

            <p
              class="mt-2 font-mono text-[10px] uppercase tracking-[0.3em] text-white/45"
            >
              To Get Started
            </p>
          </div>

          <div
            class="rounded-2xl border border-white/10 bg-white/[0.03] px-8 py-6 backdrop-blur-xl"
          >
            <p class="font-serif text-5xl text-[#c5a059]">5 Days</p>

            <p
              class="mt-2 font-mono text-[10px] uppercase tracking-[0.3em] text-white/45"
            >
              Average Funding
            </p>
          </div>
        </div>
      </div>

      <!-- RIGHT -->
      <div
        bind:this={formRef}
        class="relative overflow-hidden rounded-[38px] border border-white/10 bg-white/[0.045] p-8 shadow-[0_0_80px_rgba(0,0,0,0.55)] backdrop-blur-2xl sm:p-10"
        style="transform-style: preserve-3d;"
      >
        <!-- GLOW -->
        <div class="card-glow"></div>

        <!-- SHINE -->
        <div class="shine"></div>

        <!-- CONTENT -->
        <div class="relative z-[5]">
          <!-- PROGRESS -->
          <div
            class="mb-10 overflow-hidden rounded-full border border-white/10 bg-white/[0.03]"
          >
            <div
              class="h-[6px] rounded-full bg-gradient-to-r from-[#c5a059] via-[#f0d197] to-[#c5a059] transition-all duration-700"
              style="width:{progress}%"
            ></div>
          </div>

          <!-- TITLE -->
          <div class="mb-8">
            <p
              class="mb-4 font-mono text-[11px] uppercase tracking-[0.3em] text-white/40"
            >
              What's your top financial goal right now?
            </p>

            <h3 class="font-serif text-3xl text-white">
              Select Your Priority
            </h3>
          </div>

          <!-- OPTIONS -->
          <div class="space-y-4">
            {#each options as opt}
              {@const isActive = selected === opt.id}

              <button
                on:click={() => selectOption(opt.id)}
                class={`quiz-option relative isolate flex w-full items-center gap-5 overflow-hidden rounded-2xl border p-5 text-left transition-all duration-500 ${
                  isActive
                    ? 'border-[#c5a059]/40 bg-[#c5a059]/10 shadow-[0_0_50px_rgba(197,160,89,0.15)]'
                    : 'border-white/10 bg-white/[0.02] hover:border-[#c5a059]/20 hover:bg-white/[0.04]'
                }`}
              >
                <!-- ACTIVE GLOW -->
                {#if isActive}
                  <div class="active-glow"></div>
                {/if}

                <!-- RADIO -->
                <div
                  class={`relative z-10 flex h-6 w-6 shrink-0 items-center justify-center rounded-full border transition-all duration-500 ${
                    isActive
                      ? 'border-[#c5a059] bg-[#c5a059]'
                      : 'border-white/25'
                  }`}
                >
                  {#if isActive}
                    <div class="h-2 w-2 rounded-full bg-black"></div>
                  {/if}
                </div>

                <!-- LABEL -->
                <span
                  class={`relative z-10 text-[15px] font-medium transition-all duration-500 sm:text-base ${
                    isActive ? 'text-white' : 'text-white/65'
                  }`}
                >
                  {opt.label}
                </span>

                <!-- ICON -->
                <div
                  class={`relative z-10 ml-auto flex h-10 w-10 shrink-0 items-center justify-center rounded-full border transition-all duration-500 ${
                    isActive
                      ? 'border-[#c5a059] bg-[#c5a059] text-black'
                      : 'border-white/10 bg-white/[0.03] text-white/40'
                  }`}
                >
                  <svg
                    class="h-4 w-4"
                    fill="none"
                    viewBox="0 0 24 24"
                    stroke="currentColor"
                    stroke-width="2.5"
                  >
                    <path
                      stroke-linecap="round"
                      stroke-linejoin="round"
                      d="M5 13l4 4L19 7"
                    />
                  </svg>
                </div>
              </button>
            {/each}
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
    opacity: 0.03;
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
      rgba(0,0,0,0.82) 100%);
  }

  .mesh1,
  .mesh2 {
    position: absolute;
    border-radius: 9999px;
    filter: blur(120px);
  }

  .mesh1 {
    top: -200px;
    left: -200px;
    width: 700px;
    height: 700px;
    background: rgba(197,160,89,0.12);
  }

  .mesh2 {
    bottom: -300px;
    right: -300px;
    width: 800px;
    height: 800px;
    background: rgba(197,160,89,0.08);
  }

  .particles::before {
    content: '';
    position: absolute;
    inset: 0;

    background-image:
      radial-gradient(#c5a059 1px, transparent 1px);

    background-size: 140px 140px;

    opacity: 0.05;

    animation: drift 30s linear infinite;
  }

  @keyframes drift {
    from {
      transform: translateY(0px);
    }

    to {
      transform: translateY(-120px);
    }
  }

  .card-glow {
    position: absolute;
    inset: 0;
    z-index: 0;
    border-radius: inherit;
    pointer-events: none;

    background:
      radial-gradient(circle at top,
      rgba(197,160,89,0.18),
      transparent 60%);
  }

  .shine {
    position: absolute;
    inset: 0;
    z-index: 1;
    overflow: hidden;
    pointer-events: none;
    border-radius: inherit;
  }

  .shine::before {
    content: '';
    position: absolute;
    top: -150%;
    left: -150%;
    width: 300%;
    height: 300%;

    background:
      linear-gradient(
        115deg,
        transparent 40%,
        rgba(255,255,255,0.06) 50%,
        transparent 60%
      );

    animation: shineMove 8s linear infinite;
  }

  @keyframes shineMove {
    from {
      transform: translateX(-40%) rotate(12deg);
    }

    to {
      transform: translateX(40%) rotate(12deg);
    }
  }

  .active-glow {
    position: absolute;
    inset: 0;
    z-index: 0;
    pointer-events: none;

    background:
      radial-gradient(circle at left,
      rgba(197,160,89,0.16),
      transparent 70%);
  }
</style>