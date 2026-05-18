<script lang="ts">
  import { onMount } from 'svelte';
  import r1 from '$lib/assets/helloc/4.webp';
  import r2 from '$lib/assets/helloc/5.webp';
  import r3 from '$lib/assets/helloc/6.webp';

  let sectionRef: HTMLElement;
  let stickyRef: HTMLElement;
  let rightColRef: HTMLElement;
  let itemRefs: HTMLElement[] = [];
  let progressBarRef: HTMLElement;
  let activeIndex = $state(0);
  let progressPct = $state(0);

  const breakdown = [
    {
      title: 'Traditional Mortgage',
      subtitle: 'The Bank Controls The Timeline',
      label: '01',
      tag: 'Structure A',
      text: 'Your paycheck enters a checking account, sits idle, and your mortgage balance barely changes month-to-month. Interest accrues daily on the full outstanding balance with no relief.',
      image: r1,
      chips: ['Front-loaded Interest', 'Static Amortization', 'Bank Wins'],
      stat: '30 yrs',
      statLabel: 'avg. payoff timeline'
    },
    {
      title: 'Refinancing',
      subtitle: 'A Lower Payment Doesn\'t Mean Less Debt',
      label: '02',
      tag: 'Structure B',
      text: 'You restart the amortization schedule and often extend debt longer — even if the monthly payment looks lower on the surface. Total interest paid skyrockets.',
      image: r2,
      chips: ['Reset Clock', 'Extended Timeline', 'Hidden Cost'],
      stat: '+$40k',
      statLabel: 'avg. extra interest paid'
    },
    {
      title: 'First Lien HELOC',
      subtitle: 'Cash Flow Starts Working For You',
      label: '03',
      tag: 'Structure C',
      text: 'Income immediately offsets principal daily, reducing interest exposure while improving cash-flow flexibility. Your money works harder from the moment it arrives.',
      image: r3,
      chips: ['Daily Interest Offset', 'Equity Velocity', 'You Win'],
      stat: '7–12 yrs',
      statLabel: 'typical payoff with strategy'
    }
  ];

  onMount(async () => {
    const gsap = (await import('gsap')).default;
    const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');
    const { SplitText } = await import('gsap/dist/SplitText');
    gsap.registerPlugin(ScrollTrigger, SplitText);

    /* ── Sticky label entrance ─────────────────────────────────────── */
    gsap.from(stickyRef, {
      opacity: 0, y: 60, duration: 1.2, ease: 'power4.out',
      scrollTrigger: { trigger: sectionRef, start: 'top 70%', once: true }
    });

    /* ── Split headline ────────────────────────────────────────────── */
    const hEl = stickyRef.querySelector('h2');
    if (hEl) {
      const split = new SplitText(hEl, { type: 'lines,words', linesClass: 'clip-ln' });
      gsap.from(split.words, {
        y: '110%', opacity: 0, duration: .8, stagger: .05, ease: 'power3.out',
        scrollTrigger: { trigger: sectionRef, start: 'top 68%', once: true }
      });
    }

    /* ── Progress bar driven by scroll through right column ────────── */
    ScrollTrigger.create({
      trigger: rightColRef,
      start: 'top center',
      end: 'bottom center',
      onUpdate(self) {
        progressPct = Math.round(self.progress * 100);
      }
    });

    /* ── Per-item: image parallax + active tracking ────────────────── */
    itemRefs.forEach((el, i) => {
      if (!el) return;

      // Track which item is in center view → update activeIndex
      ScrollTrigger.create({
        trigger: el,
        start: 'top 55%',
        end: 'bottom 55%',
        onEnter: () => { activeIndex = i; },
        onEnterBack: () => { activeIndex = i; }
      });

      // Image inner parallax
      const img = el.querySelector('img');
      if (img) {
        gsap.fromTo(img,
          { scale: 1.18, filter: 'brightness(0.6) saturate(0.4)' },
          {
            scale: 1.0, filter: 'brightness(1) saturate(1)',
            duration: 1.4, ease: 'power4.out',
            scrollTrigger: { trigger: el, start: 'top 82%', once: true }
          }
        );
        // Ongoing parallax
        gsap.to(img, {
          y: -50, ease: 'none',
          scrollTrigger: {
            trigger: el,
            start: 'top bottom', end: 'bottom top',
            scrub: 1.5
          }
        });
      }

      // Text content slides in
      const text = el.querySelector('.item-text');
      if (text) {
        gsap.from(text, {
          x: 55, opacity: 0, duration: 1, ease: 'power3.out',
          scrollTrigger: { trigger: el, start: 'top 78%', once: true }
        });
      }

      // Chips stagger
      const chips = el.querySelectorAll('.chip');
      gsap.from(chips, {
        y: 18, opacity: 0, scale: .9, duration: .5,
        stagger: .08, ease: 'back.out(1.6)',
        scrollTrigger: { trigger: el, start: 'top 75%', once: true }
      });
    });

    /* ── Sticky quote blockquote fade-in lines ─────────────────────── */
    const quote = stickyRef.querySelector('blockquote');
    if (quote) {
      const qs = new SplitText(quote, { type: 'lines', linesClass: 'clip-ln' });
      gsap.from(qs.lines, {
        y: '100%', opacity: 0, duration: .7, stagger: .1, ease: 'power2.out',
        scrollTrigger: { trigger: stickyRef, start: 'top 65%', once: true }
      });
    }

    return () => { ScrollTrigger.getAll().forEach(t => t.kill()); };
  });
</script>

<style>
  /* ── Section ─────────────────────────────────────────────────────── */
  section {
  position: relative;
  overflow: visible;
  background: var(--theme-color);
  padding: 8rem 0 10rem;
	}

  /* Film grain */
  section::after {
    content: '';
    position: absolute; inset: 0; pointer-events: none; z-index: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.85' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='1'/%3E%3C/svg%3E");
    opacity: .028;
  }

  /* Orbs */
  .orb-l {
    position: absolute; left: -10%; top: 8%;
    width: 700px; height: 700px; border-radius: 50%;
    background: radial-gradient(circle, rgba(197,160,89,.09) 0%, transparent 65%);
    filter: blur(130px); pointer-events: none; z-index: 0;
  }
  .orb-r {
    position: absolute; right: -10%; bottom: -10%;
    width: 650px; height: 650px; border-radius: 50%;
    background: radial-gradient(circle, rgba(197,160,89,.06) 0%, transparent 70%);
    filter: blur(160px); pointer-events: none; z-index: 0;
  }

  /* Top rule */
  .top-rule {
    position: absolute; top: 0; left: 0; right: 0; height: 1px;
    background: linear-gradient(90deg, transparent, rgba(197,160,89,.45) 40%, rgba(197,160,89,.45) 60%, transparent);
    z-index: 1;
  }

  /* ── Main grid ────────────────────────────────────────────────────── */
  .grid-wrap {
    position: relative; z-index: 2;
    max-width: 88rem; margin: 0 auto;
    padding: 0 1.75rem;
    display: grid;
    grid-template-columns: 1fr;
    gap: 5rem;
    /* KEY: align-items start so the left col doesn't stretch to full right col height */
    align-items: start;
  }
  @media (min-width: 1024px) {
    .grid-wrap {
      grid-template-columns: 0.85fr 1.15fr;
      gap: 6rem;
    }
  }


  /* ── LEFT STICKY ─────────────────────────────────────────────────── */
  .sticky-col {
    position: sticky;
	top: 7rem;
	align-self: start;
	height: max-content;
  }

  /* Progress bar vertical */
  .progress-wrap {
    display: flex; align-items: center; gap: .75rem; margin-bottom: 2rem;
  }
  .progress-track {
    flex: 1; height: 2px;
    background: rgba(255,255,255,.07); border-radius: 2px; overflow: hidden;
  }
  .progress-fill {
    height: 100%; border-radius: 2px;
    background: linear-gradient(90deg, #c5a059, #f0d080);
    transition: width .1s ease;
    box-shadow: 0 0 8px rgba(197,160,89,.6);
  }
  .progress-pct {
    font-family: 'Courier New', monospace; font-size: .62rem;
    letter-spacing: .15em; color: rgba(197,160,89,.5);
    min-width: 2.5rem; text-align: right;
  }

  /* Eyebrow pill */
  .eyebrow-pill {
    display: inline-flex; align-items: center; gap: .65rem;
    border: 1px solid rgba(255,255,255,.09);
    background: rgba(255,255,255,.03);
    padding: .55rem 1.1rem; border-radius: 999px;
    backdrop-filter: blur(12px); -webkit-backdrop-filter: blur(12px);
    margin-bottom: 2rem;
  }
  .eyebrow-dot {
    width: 6px; height: 6px; border-radius: 50%;
    background: #c5a059;
    box-shadow: 0 0 6px rgba(197,160,89,.8);
    animation: pulse 2s infinite;
  }
  @keyframes pulse {
    0%,100% { opacity: 1; transform: scale(1); }
    50% { opacity: .5; transform: scale(.7); }
  }
  .eyebrow-text {
    font-family: 'Courier New', monospace; font-size: .65rem;
    text-transform: uppercase; letter-spacing: .28em;
    color: rgba(255,255,255,.45);
  }

  /* Headline */
  .sticky-h2 {
    font-family: Georgia, 'Times New Roman', serif;
    font-size: clamp(2.8rem, 4.5vw, 5rem);
    font-weight: 400; line-height: .95;
    letter-spacing: -.04em; color: #f2ede4;
    margin-bottom: 1.75rem;
  }
  .sticky-h2 em { font-style: italic; color: #c5a059; }
  :global(.clip-ln) { overflow: hidden; display: block; }

  /* Body */
  .sticky-body {
    font-size: 1.05rem; line-height: 1.8;
    color: rgba(255,255,255,.42);
    margin-bottom: 2.5rem; max-width: 30rem;
  }

  /* Active indicator dots */
  .step-dots {
    display: flex; gap: .6rem; margin-bottom: 2.5rem;
  }
  .step-dot {
    width: 28px; height: 3px; border-radius: 999px;
    background: rgba(255,255,255,.12);
    transition: background .4s ease, width .4s cubic-bezier(.16,1,.3,1);
  }
  .step-dot.active {
    width: 52px;
    background: linear-gradient(90deg, #c5a059, #f0d080);
    box-shadow: 0 0 8px rgba(197,160,89,.5);
  }

  /* Active label */
  .active-label {
    font-family: 'Courier New', monospace; font-size: .65rem;
    text-transform: uppercase; letter-spacing: .22em;
    color: rgba(197,160,89,.6);
    margin-bottom: .5rem;
    transition: opacity .3s;
  }
  .active-title {
    font-family: Georgia, serif; font-size: 1.1rem;
    color: rgba(255,255,255,.7); letter-spacing: -.01em;
    transition: opacity .3s;
  }

  /* Quote */
  .quote-wrap {
    margin-top: 2.5rem; padding-left: 1.25rem;
    border-left: 2px solid rgba(197,160,89,.35);
  }
  .quote-wrap blockquote {
    font-family: Georgia, 'Times New Roman', serif;
    font-size: 1.15rem; line-height: 1.6;
    letter-spacing: -.01em; color: rgba(255,255,255,.55);
    font-style: italic; margin: 0;
  }

  /* ── RIGHT COLUMN ─────────────────────────────────────────────────── */
  .right-col { display: flex; flex-direction: column; gap: 7rem; }

  /* Each item */
  .item {
    display: grid;
    grid-template-columns: 1fr;
    gap: 2.5rem;
    align-items: center;
  }
  @media (min-width: 768px) {
    .item { grid-template-columns: 1fr 1fr; }
    .item.flip .item-img  { order: 2; }
    .item.flip .item-text { order: 1; }
  }

  /* Image */
  .item-img {
    position: relative; overflow: hidden;
    border-radius: 2rem;
    border: 1px solid rgba(255,255,255,.07);
  }
  .item-img img {
    height: 480px; width: 100%; object-fit: cover;
    display: block; will-change: transform, filter;
  }
  /* Bottom gradient */
  .item-img::after {
    content: '';
    position: absolute; inset: 0;
    background: linear-gradient(to top, rgba(3,3,7,.8) 0%, rgba(3,3,7,.1) 45%, transparent 70%);
    pointer-events: none;
  }
  /* Number badge */
  .img-num {
    position: absolute; top: 1.25rem; left: 1.25rem; z-index: 2;
    font-family: 'Courier New', monospace; font-size: .65rem;
    text-transform: uppercase; letter-spacing: .28em;
    color: rgba(255,255,255,.5);
    border: 1px solid rgba(255,255,255,.1);
    background: rgba(0,0,0,.45);
    padding: .45rem 1rem; border-radius: 999px;
    backdrop-filter: blur(12px); -webkit-backdrop-filter: blur(12px);
  }
  /* Bottom stat on image */
  .img-stat {
    position: absolute; bottom: 1.5rem; left: 1.5rem; z-index: 2;
    display: flex; align-items: baseline; gap: .4rem;
  }
  .img-stat-val {
    font-family: Georgia, serif; font-size: 2.2rem; font-weight: 400;
    color: #c5a059; letter-spacing: -.04em;
  }
  .img-stat-lbl {
    font-family: 'Courier New', monospace; font-size: .58rem;
    text-transform: uppercase; letter-spacing: .18em;
    color: rgba(255,255,255,.4);
  }

  /* Text */
  .item-text {}
  .item-tag {
    font-family: 'Courier New', monospace; font-size: .65rem;
    text-transform: uppercase; letter-spacing: .28em;
    color: rgba(197,160,89,.7); margin-bottom: 1.25rem;
  }
  .item-h3 {	
    font-family: Georgia, 'Times New Roman', serif;
    font-size: clamp(2rem, 3.5vw, 2.8rem);
    font-weight: 400; line-height: 1.05;
    letter-spacing: -.035em; color: #f2ede4;
    margin-bottom: 1.25rem;
  }
  .item-body {
    font-size: .97rem; line-height: 1.8;
    color: rgba(255,255,255,.42);
    margin-bottom: 1.75rem;
  }

  /* Gold rule */
  .item-rule {
    height: 1px; max-width: 360px;
    background: linear-gradient(90deg, rgba(197,160,89,.35), transparent);
    margin-bottom: 1.5rem;
  }

  /* Chips */
  .chips { display: flex; flex-wrap: wrap; gap: .5rem; }
  .chip {
    font-size: .72rem; letter-spacing: .04em;
    color: rgba(255,255,255,.5);
    border: 1px solid rgba(255,255,255,.09);
    background: rgba(255,255,255,.03);
    padding: .4rem 1rem; border-radius: 999px;
    backdrop-filter: blur(8px); -webkit-backdrop-filter: blur(8px);
    transition: border-color .25s, color .25s;
  }
  .chip:hover { border-color: rgba(197,160,89,.3); color: rgba(197,160,89,.8); }

  .section-visuals {
	position: absolute;
	inset: 0;
	overflow: hidden;
	pointer-events: none;
	z-index: 0;
	}
</style>

<section bind:this={sectionRef}>
	<div class="section-visuals">
		<div class="top-rule"></div>
		<div class="orb-l"></div>
		<div class="orb-r"></div>
	</div>

  <div class="grid-wrap">

    <!-- ══ LEFT — sticky ════════════════════════════════════════════ -->
    <div class="sticky-col sticky ">
      <div bind:this={stickyRef}>

        <!-- Progress bar -->
        <div class="progress-wrap">
          <div class="progress-track">
            <div class="progress-fill" style="width: {progressPct}%"></div>
          </div>
          <span class="progress-pct">{progressPct}%</span>
        </div>

        <!-- Eyebrow -->
        <div class="eyebrow-pill">
          <span class="eyebrow-dot"></span>
          <span class="eyebrow-text">Compare The Structures</span>
        </div>

        <!-- Headline -->
        <h2 class="sticky-h2">
          Three<br />
          <em>Completely</em><br />
          Different<br />
          Outcomes
        </h2>

        <!-- Body -->
        <p class="sticky-body">
          The same homeowner can end up in entirely different financial positions — depending on the structure they choose.
        </p>

        <!-- Active indicator -->
        <div class="step-dots">
          {#each breakdown as _, i}
            <div class="step-dot {activeIndex === i ? 'active' : ''}"></div>
          {/each}
        </div>
        <p class="active-label">{breakdown[activeIndex].tag}</p>
        <p class="active-title">{breakdown[activeIndex].subtitle}</p>

        <!-- Quote -->
        <div class="quote-wrap">
          <blockquote>
            "Financial freedom isn't about income alone. It's about how your cash flow is structured."
          </blockquote>
        </div>

      </div>
    </div>

    <!-- ══ RIGHT — scrolling items ══════════════════════════════════ -->
    <div class="right-col" bind:this={rightColRef}>
      {#each breakdown as item, i}
        <div
          class="item {i % 2 !== 0 ? 'flip' : ''}"
          bind:this={itemRefs[i]}
        >

          <!-- Image -->
          <div class="item-img">
            <img src={item.image} alt={item.title} loading="lazy" />
            <div class="img-num">{item.label}</div>
            <div class="img-stat">
              <span class="img-stat-val">{item.stat}</span>
              <span class="img-stat-lbl">{item.statLabel}</span>
            </div>
          </div>

          <!-- Text -->
          <div class="item-text">
            <p class="item-tag">{item.tag} — {item.title}</p>
            <h3 class="item-h3">{item.subtitle}</h3>
            <p class="item-body">{item.text}</p>
            <div class="item-rule"></div>
            <div class="chips">
              {#each item.chips as chip}
                <span class="chip">{chip}</span>
              {/each}
            </div>
          </div>

        </div>
      {/each}
    </div>

  </div>
</section>