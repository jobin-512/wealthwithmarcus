<script lang="ts">
  // @ts-nocheck

  import { onMount } from 'svelte';

  let sectionRef: HTMLElement;
  let spotlightRef: HTMLElement;
  let headlineRef: HTMLElement;

  let columns: HTMLElement[] = [];

  const categories = [
    {
      title: 'Home & Mortgage Solutions',
      items: [
        { name: 'Conventional Loans', desc: 'Fixed and adjustable-rate options for primary residences.' },
        { name: 'FHA', desc: 'Low down-payment government-backed home loans.' },
        { name: 'VA (Purchased & REFI)', desc: 'Zero-down financing for veterans and active military.' },
        { name: 'Jumbo Loans', desc: 'High-value mortgages that exceed conforming limits.' },
        { name: 'Bank Statement Loans', desc: 'For self-employed borrowers using bank deposits.' },
        { name: 'DSCR (for Investors)', desc: 'Qualify based on property cash flow, not personal income.' },
        { name: 'Fast Digital HELOCs', desc: 'Tap home equity quickly with a digital-first process.' },
        { name: 'Cash-out REFI', desc: 'Access equity while refinancing your current mortgage.' },
        { name: 'Rate & Term REFI', desc: 'Lower your rate or change your term without cash out.' },
        { name: 'HECM (Reverse Mortgage)', desc: 'Home equity conversion for homeowners 62+.' },
        { name: 'SecureEquity+', desc: 'Proprietary reverse HELOC and 2nd mortgage solutions.' }
      ]
    },
    {
      title: 'Business Financing',
      items: [
        { name: 'Unsecured Business Loans', desc: 'Capital without collateral for qualified businesses.' },
        { name: 'Business Lines of Credit', desc: 'Revolving access to funds when you need them.' },
        { name: 'SBA Loans', desc: 'Government-guaranteed financing with competitive terms.' },
        { name: 'Working Capital Loans', desc: 'Short-term funding for daily operations.' },
        { name: 'Business Acquisition Financing', desc: 'Funding to buy an existing business or franchise.' },
        { name: 'Accounts Receivable Financing', desc: 'Leverage outstanding invoices for immediate cash.' },
        { name: 'Equipment Leasing Financing', desc: 'Preserve cash flow while acquiring essential equipment.' },
        { name: 'Securities-based Lending', desc: 'Borrow against your investment portfolio.' },
        { name: 'Revenue-based Financing', desc: 'Growth capital repaid as a percentage of revenue.' }
      ]
    },
    {
      title: 'Investor & Commercial Solutions',
      items: [
        { name: 'Fix-and-Flip', desc: 'Short-term funding for renovation and resale projects.' },
        { name: 'Commercial Real-estate', desc: 'Financing for office, retail, and industrial properties.' },
        { name: 'Bridge Loans', desc: 'Interim financing to bridge gaps between transactions.' },
        { name: 'DSCR (for Investors)', desc: 'Investment property loans based on debt-service coverage.' },
        { name: 'Multi-family and Rental Property Financing', desc: 'Long-term loans for duplexes to apartment buildings.' },
        { name: 'Church & Non-profit Financing', desc: 'Specialized lending for religious and 501(c)(3) entities.' }
      ]
    }
  ];

  const headlineParts = [
    { text: 'One', break: false },
    { text: 'Partner.', break: true },
    { text: 'Every', accent: true },
    { text: 'Funding', accent: true },
    { text: 'Solution', accent: true },
    { text: 'You', accent: true },
    { text: 'Need.', accent: true }
  ];

  let activeItem: string | null = $state(null);

  function toggleItem(name: string) {
    activeItem = activeItem === name ? null : name;
  }

  onMount(async () => {
    const gsapModule = await import('gsap');
    const gsap = gsapModule.default;

    const scrollModule = await import('gsap/dist/ScrollTrigger');
    const ScrollTrigger = scrollModule.ScrollTrigger;

    gsap.registerPlugin(ScrollTrigger);

    // Spotlight movement
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

    // HEADLINE
    const chars = headlineRef.querySelectorAll('.char');

    gsap.from(chars, {
      y: 120,
      opacity: 0,
      rotateX: -90,
      stagger: 0.03,
      duration: 1.2,
      ease: 'power4.out',
      scrollTrigger: {
        trigger: headlineRef,
        start: 'top 85%'
      }
    });

    // COLUMNS
    gsap.from(columns, {
      opacity: 0,
      y: 100,
      stagger: 0.15,
      duration: 1.2,
      ease: 'power4.out',
      scrollTrigger: {
        trigger: sectionRef,
        start: 'top 70%'
      }
    });

    // FLOATING
    columns.forEach((col) => {
      col.addEventListener('mouseenter', () => {
        gsap.to(col, {
          y: -6,
          duration: 0.45,
          ease: 'power3.out'
        });
      });

      col.addEventListener('mouseleave', () => {
        gsap.to(col, {
          y: 0,
          duration: 0.6,
          ease: 'power3.out'
        });
      });
    });

    // MAGNETIC
    columns.forEach((col) => {
      col.addEventListener('mousemove', (e: MouseEvent) => {
        const rect = col.getBoundingClientRect();

        const x = e.clientX - rect.left;
        const y = e.clientY - rect.top;

        gsap.to(col, {
          rotateY: gsap.utils.mapRange(0, rect.width, -5, 5, x),
          rotateX: gsap.utils.mapRange(0, rect.height, 5, -5, y),
          transformPerspective: 1200,
          duration: 0.5,
          ease: 'power3.out'
        });
      });

      col.addEventListener('mouseleave', () => {
        gsap.to(col, {
          rotateX: 0,
          rotateY: 0,
          duration: 1,
          ease: 'elastic.out(1,0.5)'
        });
      });
    });

    return () => {
      sectionRef?.removeEventListener('mousemove', moveSpotlight);
      ScrollTrigger.getAll().forEach((t) => t.kill());
    };
  });
</script>

<section
  bind:this={sectionRef}
  class="relative overflow-hidden bg-[#050505] py-32 lg:py-44"
>
  <!-- Background -->
  <div class="noise"></div>
  <div class="mesh-bg"></div>
  <div class="vignette"></div>

  <!-- Spotlight -->
  <div
    bind:this={spotlightRef}
    class="spotlight"
  ></div>

  <!-- Lines -->
  <svg
    class="pointer-events-none absolute inset-0 h-full w-full opacity-[0.04]"
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
    <!-- Heading -->
    <div class="mb-24 text-center lg:mb-32">
      <p
        class="mb-7 font-mono text-[11px] uppercase tracking-[0.45em] text-white/45"
      >
        Flexible funding, faster approvals, and solutions designed around you.
      </p>

      <h2
        bind:this={headlineRef}
        class="font-serif text-[clamp(3rem,6vw,6rem)] font-medium leading-[0.95] tracking-[-0.03em] text-white"
      >
        {#each headlineParts as part}
          <span
            class="char mr-[0.18em] inline-block"
            class:italic={part.accent}
            class:text-[#c5a059]={part.accent}
          >
            {part.text}
          </span>

          {#if part.break}
            <br class="hidden sm:block" />
          {/if}
        {/each}
      </h2>
    </div>

    <!-- Grid -->
    <div class="grid grid-cols-1 gap-8 lg:grid-cols-3">
      {#each categories as cat, ci}
        <div
          bind:this={columns[ci]}
          class="card group"
        >
          <!-- Glow -->
          <div class="card-glow"></div>

          <!-- Shine -->
          <div class="shine"></div>

          <h3
            class="relative mb-8 border-b border-white/10 pb-5 font-mono text-[11px] uppercase tracking-[0.28em] text-[#c5a059]"
          >
            {cat.title}
          </h3>

          <div>
            {#each cat.items as item}
              <div
                class="loan-item border-b border-white/6"
              >
                {#if activeItem === item.name}
                  <div class="active-bar"></div>
                {/if}

                <!-- svelte-ignore event_directive_deprecated -->
                <button
                  on:click={() => toggleItem(item.name)}
                  class="item-btn"
                >
                  <span
                    class="item-title"
                  >
                    {item.name}
                  </span>

                  <span
                    class="icon-wrap"
                    class:active={activeItem === item.name}
                  >
                    <svg
                      class="h-3 w-3"
                      fill="none"
                      viewBox="0 0 24 24"
                      stroke="currentColor"
                      stroke-width="2.5"
                    >
                      <path
                        stroke-linecap="round"
                        stroke-linejoin="round"
                        d="M19 9l-7 7-7-7"
                      />
                    </svg>
                  </span>
                </button>

                {#if activeItem === item.name}
                  <div class="desc-wrap">
                    <p class="desc">
                      {item.desc}
                    </p>
                  </div>
                {/if}
              </div>
            {/each}
          </div>
        </div>
      {/each}
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
      radial-gradient(rgba(255,255,255,0.25) 0.5px, transparent 0.5px);
    background-size: 4px 4px;
  }

  .mesh-bg {
    position: absolute;
    inset: -20%;
    filter: blur(120px);

    background:
      radial-gradient(circle at 20% 20%, rgba(197,160,89,0.12), transparent 25%),
      radial-gradient(circle at 80% 30%, rgba(197,160,89,0.08), transparent 35%),
      radial-gradient(circle at 50% 80%, rgba(197,160,89,0.08), transparent 30%);

    animation: meshMove 18s ease-in-out infinite alternate;
  }

  .vignette {
    position: absolute;
    inset: 0;

    background:
      radial-gradient(circle at center,
      transparent 20%,
      rgba(0,0,0,0.72) 100%);
  }

  .spotlight {
    position: absolute;
    left: 0;
    top: 0;
    width: 500px;
    height: 500px;
    border-radius: 9999px;

    background: rgba(197,160,89,0.08);

    filter: blur(120px);

    pointer-events: none;
  }

  .card {
    position: relative;
    overflow: hidden;

    padding: 32px;

    border-radius: 32px;

    border: 1px solid rgba(255,255,255,0.08);

    background: rgba(255,255,255,0.03);

    backdrop-filter: blur(24px);

    transition:
      border-color 0.5s ease,
      transform 0.5s ease,
      box-shadow 0.5s ease;
  }

  .card:hover {
    border-color: rgba(197,160,89,0.35);

    box-shadow:
      0 0 80px rgba(197,160,89,0.08);
  }

  .card-glow {
    position: absolute;
    inset: 0;

    opacity: 0;

    transition: opacity 0.6s ease;

    background:
      radial-gradient(circle at top,
      rgba(197,160,89,0.12),
      transparent 70%);
  }

  .card:hover .card-glow {
    opacity: 1;
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

    animation: shineMove 8s linear infinite;

    pointer-events: none;
  }

  .loan-item {
    position: relative;
  }

  .active-bar {
    position: absolute;
    left: 0;
    top: 0;
    bottom: 0;

    width: 2px;

    background: #c5a059;
  }

  .item-btn {
    width: 100%;

    display: flex;
    align-items: center;
    justify-content: space-between;

    gap: 16px;

    padding: 20px 0;

    text-align: left;

    transition: all 0.4s ease;
  }

  .item-title {
    color: white;
    font-size: 15px;
    font-weight: 500;
    transition: all 0.4s ease;
  }

  .loan-item:hover .item-title {
    color: #e7c98d;
    transform: translateX(6px);
  }

  .icon-wrap {
    width: 32px;
    height: 32px;

    display: flex;
    align-items: center;
    justify-content: center;

    border-radius: 9999px;

    border: 1px solid rgba(255,255,255,0.1);

    background: rgba(255,255,255,0.03);

    color: white;

    transition: all 0.4s ease;
  }

  .icon-wrap.active {
    background: #c5a059;
    color: black;
    border-color: #c5a059;
    transform: rotate(180deg);
  }

  .desc-wrap {
    overflow: hidden;

    animation: fadeUp 0.4s ease;
  }

  .desc {
    padding: 0 0 24px 14px;

    color: rgba(255,255,255,0.55);

    font-size: 14px;
    line-height: 1.7;
  }

  @keyframes meshMove {
    from {
      transform: translateY(0px) scale(1);
    }

    to {
      transform: translateY(-60px) scale(1.08);
    }
  }

  @keyframes shineMove {
    from {
      transform: translateX(-100%) rotate(12deg);
    }

    to {
      transform: translateX(100%) rotate(12deg);
    }
  }

  @keyframes fadeUp {
    from {
      opacity: 0;
      transform: translateY(10px);
      filter: blur(6px);
    }

    to {
      opacity: 1;
      transform: translateY(0px);
      filter: blur(0px);
    }
  }
</style>