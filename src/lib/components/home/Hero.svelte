<script>
// @ts-nocheck
  import { onMount } from 'svelte';
  import crystal from "$lib/assets/home/banner.mp4";

  let sectionRef, canvasRef, videoRef;
  let eyebrowRef, h1Ref, subRef, ctaRef, ctaInnerRef, badgesRef, tickerRef;
  let orb1, orb2, orb3, lineL, lineR, scrollEl;
  let badge1, badge2, badge3;

  /* ── Magnetic CTA ───────────────────────────────────────────────── */
  function onCtaMove(e) {
    if (!ctaInnerRef) return;
    const r = ctaInnerRef.getBoundingClientRect();
    const dx = (e.clientX - r.left - r.width  / 2) * 0.32;
    const dy = (e.clientY - r.top  - r.height / 2) * 0.32;
    ctaInnerRef.style.transform = `translate(${dx}px,${dy}px)`;
  }
  function onCtaLeave() {
    if (ctaInnerRef) ctaInnerRef.style.transform = 'translate(0,0)';
  }

  onMount(async () => {
    const gsap = (await import('gsap')).default;
    const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');
    const { SplitText }     = await import('gsap/dist/SplitText');
    gsap.registerPlugin(ScrollTrigger, SplitText);

    /* ── Particle canvas ──────────────────────────────────────────── */
    const cv = canvasRef;
    const ctx = cv.getContext('2d');
    let W = 0, H = 0;
    const resize = () => { W = cv.offsetWidth; H = cv.offsetHeight; cv.width = W; cv.height = H; };
    resize();
    window.addEventListener('resize', resize);

    const G = 'rgba(197,160,89,';
    const pts = Array.from({ length: 70 }, () => ({
      x: Math.random() * 1600, y: Math.random() * 900,
      vx: (Math.random() - .5) * .25, vy: (Math.random() - .5) * .25,
      r: Math.random() * 1.2 + .25,
      pa: Math.random() * Math.PI * 2,
      ps: (.002 + Math.random() * .004) * (Math.random() > .5 ? 1 : -1),
      a: 0
    }));

    let raf;
    const tick = () => {
      ctx.clearRect(0, 0, W, H);
      pts.forEach((p, i) => {
        p.pa += p.ps; p.a = ((Math.sin(p.pa) + 1) / 2) * .5 + .03;
        p.x += p.vx; p.y += p.vy;
        if (p.x < 0) p.x = W; if (p.x > W) p.x = 0;
        if (p.y < 0) p.y = H; if (p.y > H) p.y = 0;
        for (let j = i + 1; j < pts.length; j++) {
          const dx = p.x - pts[j].x, dy = p.y - pts[j].y;
          const d = Math.sqrt(dx*dx + dy*dy);
          if (d < 130) {
            ctx.beginPath();
            ctx.strokeStyle = G + (.06 * (1 - d / 130)) + ')';
            ctx.lineWidth = .4;
            ctx.moveTo(p.x, p.y); ctx.lineTo(pts[j].x, pts[j].y); ctx.stroke();
          }
        }
        ctx.beginPath(); ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
        ctx.fillStyle = G + p.a + ')'; ctx.fill();
      });
      raf = requestAnimationFrame(tick);
    };
    tick();

    /* ── Orb floats ───────────────────────────────────────────────── */
    gsap.to(orb1, { y: -80, x: 40, duration: 9,  repeat: -1, yoyo: true, ease: 'sine.inOut' });
    gsap.to(orb2, { y: 60,  x: -50,duration: 11, repeat: -1, yoyo: true, ease: 'sine.inOut' });
    gsap.to(orb3, { y: -50, x: 30, duration: 7,  repeat: -1, yoyo: true, ease: 'sine.inOut' });

    /* ── Parallax on scroll ───────────────────────────────────────── */
    gsap.to(videoRef, {
      y: 120, scale: 1.08, ease: 'none',
      scrollTrigger: { trigger: sectionRef, start: 'top top', end: 'bottom top', scrub: 1.5 }
    });
    gsap.to(orb1, {
      y: 200, ease: 'none',
      scrollTrigger: { trigger: sectionRef, start: 'top top', end: 'bottom top', scrub: 2 }
    });
    gsap.to(h1Ref, {
      y: 60, ease: 'none',
      scrollTrigger: { trigger: sectionRef, start: 'top top', end: 'bottom top', scrub: 1 }
    });

    /* ── Master entry timeline ────────────────────────────────────── */
    const SC = '!<>—_\\/[]{}=+*^?#ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789@$%';
    const tl = gsap.timeline({ delay: .15 });

    // 1. Horizontal lines wipe in from center
    tl.fromTo([lineL, lineR],
      { scaleX: 0 },
      { scaleX: 1, duration: .9, ease: 'expo.out', stagger: 0 },
      0
    );

    // 2. Eyebrow
    tl.fromTo(eyebrowRef,
      { opacity: 0, y: 14, letterSpacing: '0.5em' },
      { opacity: 1, y: 0,  letterSpacing: '0.25em', duration: .7, ease: 'power3.out' },
      .4
    );

    // 3. Headline — SplitText char scramble
    const hSplit = new SplitText(h1Ref, { type: 'chars,words,lines', linesClass: 'clip-l' });

    function scrambleChar(el, final, delay) {
      let n = 0, max = 14;
      setTimeout(() => {
        const id = setInterval(() => {
          if (n >= max) { el.textContent = final; clearInterval(id); return; }
          el.textContent = n > max * .6
            ? final
            : SC[Math.floor(Math.random() * SC.length)];
          n++;
        }, 35);
      }, delay);
    }

    const origChars = hSplit.chars.map(c => c.textContent);
    tl.fromTo(hSplit.chars,
      { y: '115%', rotateX: -60, opacity: 0, filter: 'blur(6px)' },
      {
        y: '0%', rotateX: 0, opacity: 1, filter: 'blur(0px)',
        duration: .75, stagger: { amount: .55 }, ease: 'back.out(1.4)',
        onStart() {
          hSplit.chars.forEach((c, i) => scrambleChar(c, origChars[i], i * 28));
        }
      },
      .6
    );

    // 4. Subtitle word reveal
    const subSplit = new SplitText(subRef, { type: 'words' });
    tl.fromTo(subSplit.words,
      { opacity: 0, y: 22 },
      { opacity: 1, y: 0, duration: .5, stagger: { amount: .5 }, ease: 'power2.out' },
      1.15
    );

    // 5. CTA bounce in
    tl.fromTo(ctaRef,
      { scale: .75, opacity: 0 },
      { scale: 1, opacity: 1, duration: .65, ease: 'back.out(2.2)' },
      1.45
    );

    // 6. Floating badges stagger in
    tl.fromTo([badge1, badge2, badge3].filter(Boolean),
      { opacity: 0, y: 30, scale: .85 },
      { opacity: 1, y: 0,  scale: 1, duration: .6, stagger: .12, ease: 'back.out(1.8)' },
      1.35
    );

    // 7. Scroll indicator
    tl.fromTo(scrollEl,
      { opacity: 0, y: 10 },
      { opacity: 1, y: 0, duration: .5, ease: 'power2.out' },
      1.8
    );

    /* ── Badge float loops ────────────────────────────────────────── */
    gsap.to(badge1, { y: -10, duration: 3.2, repeat: -1, yoyo: true, ease: 'sine.inOut', delay: 0 });
    gsap.to(badge2, { y: -14, duration: 4.1, repeat: -1, yoyo: true, ease: 'sine.inOut', delay: .7 });
    gsap.to(badge3, { y: -8,  duration: 2.9, repeat: -1, yoyo: true, ease: 'sine.inOut', delay: 1.4 });

    /* ── Scroll indicator pulse ───────────────────────────────────── */
    gsap.to(scrollEl?.querySelector('.pulse-dot'), {
      y: 10, opacity: 0, duration: 1.1, repeat: -1, ease: 'power2.inOut'
    });

    /* ── Infinite ticker ──────────────────────────────────────────── */
    const track = tickerRef?.querySelector('.ticker-track');
    if (track) {
      // Clone for seamless loop
      const clone = track.cloneNode(true);
      tickerRef.appendChild(clone);
      gsap.to(tickerRef.querySelectorAll('.ticker-track'), {
        x: '-50%', duration: 22, ease: 'none', repeat: -1
      });
    }

    return () => {
      cancelAnimationFrame(raf);
      ScrollTrigger.getAll().forEach(t => t.kill());
      window.removeEventListener('resize', resize);
    };
  });
</script>

<style>
  section {
    position: relative;
    min-height: 100svh;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    overflow: hidden;
    background: var(--theme-color);
  }

  /* Film grain */
  section::before {
    content: '';
    position: absolute; inset: 0; z-index: 3; pointer-events: none;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='.88' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='1'/%3E%3C/svg%3E");
    opacity: .04;
    mix-blend-mode: overlay;
  }

  /* ── Video ── */
  .video-wrap {
    position: absolute; inset: 0; z-index: 0;
    will-change: transform;
  }
  .video-wrap video {
    position: absolute; inset: 0;
    width: 100%; height: 100%; object-fit: cover;
    opacity: .38;
    mix-blend-mode: luminosity;
  }
  /* Radial vignette over video */
  .video-wrap::after {
    content: '';
    position: absolute; inset: 0;
    background: radial-gradient(ellipse at 50% 50%, transparent 30%, rgba(4,4,10,.92) 85%);
  }

  /* ── Particle canvas ── */
  canvas {
    position: absolute; inset: 0; width: 100%; height: 100%;
    pointer-events: none; z-index: 1;
  }

  /* ── Orbs ── */
  .orb { position: absolute; border-radius: 50%; pointer-events: none; filter: blur(90px); }
  .o1 { left: -15%; top: 10%; width: 700px; height: 700px; background: radial-gradient(circle, rgba(197,160,89,.12) 0%, transparent 65%); }
  .o2 { right: -12%; bottom: 0%; width: 600px; height: 600px; background: radial-gradient(circle, rgba(197,160,89,.08) 0%, transparent 70%); }
  .o3 { left: 40%; top: -15%; width: 500px; height: 500px; background: radial-gradient(circle, rgba(197,160,89,.05) 0%, transparent 70%); filter: blur(120px); }

  /* ── Horizontal rule lines ── */
  .line-l, .line-r {
    position: absolute; top: 50%; height: 1px;
    background: linear-gradient(90deg, transparent, rgba(197,160,89,.25));
    transform-origin: right center;
    z-index: 2;
  }
  .line-l { left: 0; right: 50%; transform-origin: left center;
    background: linear-gradient(to right, transparent, rgba(197,160,89,.25));
  }
  .line-r { left: 50%; right: 0; transform-origin: right center;
    background: linear-gradient(to left, transparent, rgba(197,160,89,.25));
  }

  /* ── Content ── */
  .content {
    position: relative; z-index: 10;
    max-width: 80rem; width: 100%;
    padding: 0 1.75rem;
    text-align: center;
    padding-top: 7rem;
  }

  /* ── Eyebrow ── */
  .eyebrow {
    font-family: 'Courier New', monospace;
    font-size: .7rem; letter-spacing: .25em; text-transform: uppercase;
    color: rgba(197,160,89,.65);
    display: flex; align-items: center; justify-content: center; gap: .75rem;
    margin-bottom: 2.5rem;
  }
  .eyebrow::before, .eyebrow::after {
    content: ''; display: block; height: 1px; width: 2.5rem;
    background: rgba(197,160,89,.35);
  }

  /* ── Headline ── */
  .h1 {
    font-family: Georgia, 'Times New Roman', serif;
    font-size: clamp(3.6rem, 9vw, 9rem);
    font-weight: 400; line-height: .97;
    letter-spacing: -.04em;
    color: #f2ede4;
    perspective: 1200px;
    margin-bottom: 2rem;
  }
  .h1 :global(.clip-l) { overflow: hidden; display: block; }
  .h1 em {
    font-style: italic; color: #c5a059;
    display: inline-block;
  }

  /* Accent underline on italic span */
  .h1-accent {
    position: relative; display: inline-block;
  }
  .h1-accent::after {
    content: '';
    position: absolute; left: 0; bottom: -.08em; right: 0; height: .05em;
    background: linear-gradient(90deg, rgba(197,160,89,.5), rgba(197,160,89,.1));
    border-radius: 4px;
  }

  /* ── Sub ── */
  .sub {
    font-size: clamp(.95rem, 2vw, 1.2rem);
    line-height: 1.8; color: rgba(255,255,255,.4);
    max-width: 38rem; margin: 0 auto 3rem;
  }
  .sub strong { color: rgba(255,255,255,.75); font-weight: 500; }

  /* ── CTA wrap ── */
  .cta-wrap {
    display: flex; align-items: center; justify-content: center; gap: 1rem; flex-wrap: wrap;
    margin-bottom: 5rem;
  }

  /* Primary CTA */
  .cta-primary {
    display: inline-flex; align-items: center; gap: .6rem;
    background: linear-gradient(135deg, #c5a059, #9a7430);
    color: #04040a; font-weight: 700; font-size: .82rem;
    letter-spacing: .09em; text-transform: uppercase;
    padding: .95rem 2.25rem; border-radius: 999px;
    text-decoration: none; position: relative; overflow: hidden;
    transition: box-shadow .35s ease;
    will-change: transform;
    transition-property: box-shadow, transform;
    transition-duration: .35s, .35s;
    transition-timing-function: ease, cubic-bezier(.23,1,.32,1);
  }
  .cta-primary:hover { box-shadow: 0 0 80px -6px rgba(197,160,89,.7); }
  .cta-shine {
    position: absolute; inset: 0;
    background: linear-gradient(105deg,transparent 35%,rgba(255,255,255,.28) 50%,transparent 65%);
    transform: translateX(-100%); transition: transform .65s ease;
  }
  .cta-primary:hover .cta-shine { transform: translateX(100%); }
  .cta-primary svg { width: 15px; height: 15px; stroke: currentColor; fill: none; stroke-width: 2.5; stroke-linecap: round; stroke-linejoin: round; position: relative; z-index: 1; }
  .cta-primary span { position: relative; z-index: 1; }

  /* Ghost CTA */
  .cta-ghost {
    display: inline-flex; align-items: center; gap: .55rem;
    color: rgba(255,255,255,.5); font-size: .82rem; font-weight: 500;
    letter-spacing: .04em; text-decoration: none;
    border: 1px solid rgba(255,255,255,.12);
    padding: .95rem 2rem; border-radius: 999px;
    transition: color .25s, border-color .25s;
    backdrop-filter: blur(6px); -webkit-backdrop-filter: blur(6px);
    background: rgba(255,255,255,.03);
  }
  .cta-ghost:hover { color: rgba(255,255,255,.85); border-color: rgba(255,255,255,.28); }
  .cta-ghost svg { width: 14px; height: 14px; stroke: currentColor; fill: none; stroke-width: 2; stroke-linecap: round; stroke-linejoin: round; }

  /* ── Floating badges ── */
  .badges {
    position: absolute; inset: 0; z-index: 5; pointer-events: none;
  }
  .badge {
    position: absolute;
    display: flex; align-items: center; gap: .6rem;
    background: rgba(10,10,14,.82);
    border: 1px solid rgba(197,160,89,.18);
    border-radius: .85rem; padding: .65rem 1rem;
    backdrop-filter: blur(16px); -webkit-backdrop-filter: blur(16px);
    box-shadow: 0 16px 48px -8px rgba(0,0,0,.5);
    will-change: transform;
  }
  .badge-icon {
    width: 2rem; height: 2rem; border-radius: .45rem; flex-shrink: 0;
    background: rgba(197,160,89,.1); border: 1px solid rgba(197,160,89,.22);
    display: flex; align-items: center; justify-content: center;
  }
  .badge-icon svg { width: 13px; height: 13px; stroke: rgba(197,160,89,.85); fill: none; stroke-width: 1.8; stroke-linecap: round; stroke-linejoin: round; }
  .badge-text {}
  .badge-val  { font-family: Georgia, serif; font-size: 1rem; font-weight: 400; color: #f2ede4; line-height: 1.1; }
  .badge-lbl  { font-family: 'Courier New', monospace; font-size: .58rem; text-transform: uppercase; letter-spacing: .16em; color: rgba(197,160,89,.55); margin-top: .1rem; }

  /* Badge positions — desktop */
  .b1 { left: 4%; top: 28%; }
  .b2 { right: 4%; top: 35%; }
  .b3 { left: 6%; bottom: 28%; }
  @media (max-width: 768px) {
    .b1, .b2, .b3 { display: none; }
  }

  /* ── Ticker ── */
  .ticker-outer {
    position: absolute; bottom: 5.5rem; left: 0; right: 0;
    z-index: 6; overflow: hidden;
    border-top: 1px solid rgba(255,255,255,.04);
    border-bottom: 1px solid rgba(255,255,255,.04);
    padding: .6rem 0;
  }
  .ticker-inner {
    display: flex; width: max-content;
  }
  .ticker-track {
    display: flex; align-items: center; gap: 0; white-space: nowrap;
  }
  .tick-item {
    display: inline-flex; align-items: center; gap: .75rem;
    padding: 0 2rem;
    font-family: 'Courier New', monospace; font-size: .65rem;
    text-transform: uppercase; letter-spacing: .2em;
    color: rgba(255,255,255,.2);
  }
  .tick-dot {
    width: 3px; height: 3px; border-radius: 50%;
    background: rgba(197,160,89,.5); flex-shrink: 0;
  }
  .tick-accent { color: rgba(197,160,89,.45); }

  /* ── Scroll indicator ── */
  .scroll-ind {
    position: absolute; bottom: 2rem; left: 50%; transform: translateX(-50%);
    z-index: 6; display: flex; flex-direction: column; align-items: center; gap: .5rem;
  }
  .scroll-text {
    font-family: 'Courier New', monospace; font-size: .58rem;
    text-transform: uppercase; letter-spacing: .2em;
    color: rgba(255,255,255,.2);
  }
  .scroll-pill {
    width: 1.4rem; height: 2.4rem; border-radius: 999px;
    border: 1px solid rgba(255,255,255,.12);
    display: flex; justify-content: center; align-items: flex-start;
    padding-top: .35rem;
  }
  .pulse-dot {
    width: 5px; height: 5px; border-radius: 50%;
    background: rgba(197,160,89,.7);
  }

  /* ── Corner accents ── */
  .corner {
    position: absolute; z-index: 4; pointer-events: none;
    width: 3rem; height: 3rem;
  }
  .corner-tl { top: 1.5rem; left: 1.5rem; border-top: 1px solid rgba(197,160,89,.25); border-left: 1px solid rgba(197,160,89,.25); }
  .corner-tr { top: 1.5rem; right: 1.5rem; border-top: 1px solid rgba(197,160,89,.25); border-right: 1px solid rgba(197,160,89,.25); }
  .corner-bl { bottom: 1.5rem; left: 1.5rem; border-bottom: 1px solid rgba(197,160,89,.25); border-left: 1px solid rgba(197,160,89,.25); }
  .corner-br { bottom: 1.5rem; right: 1.5rem; border-bottom: 1px solid rgba(197,160,89,.25); border-right: 1px solid rgba(197,160,89,.25); }
</style>

<section bind:this={sectionRef}>

  <!-- Video -->
  <div class="video-wrap" bind:this={videoRef}>
    <!-- svelte-ignore a11y_media_has_caption -->
    <video src={crystal} autoplay loop muted playsinline></video>
  </div>

  <!-- Particles -->
  <canvas bind:this={canvasRef}></canvas>

  <!-- Orbs -->
  <div class="orb o1" bind:this={orb1}></div>
  <div class="orb o2" bind:this={orb2}></div>
  <div class="orb o3" bind:this={orb3}></div>

  <!-- Horizontal center lines -->
  <div class="line-l" bind:this={lineL}></div>
  <div class="line-r" bind:this={lineR}></div>

  <!-- Corner accents -->
  <div class="corner corner-tl"></div>
  <div class="corner corner-tr"></div>
  <div class="corner corner-bl"></div>
  <div class="corner corner-br"></div>

  <!-- Floating stat badges -->
  <div class="badges" bind:this={badgesRef}>

    <div class="badge b1" bind:this={badge1}>
      <div class="badge-icon">
        <svg viewBox="0 0 24 24"><path d="M17 21v-2a4 4 0 0 0-4-4H5a4 4 0 0 0-4 4v2M9 11a4 4 0 1 0 0-8 4 4 0 0 0 0 8zm13 10v-2a4 4 0 0 0-3-3.87M16 3.13a4 4 0 0 1 0 7.75"/></svg>
      </div>
      <div class="badge-text">
        <p class="badge-val">90+ Lenders</p>
        <p class="badge-lbl">Network Access</p>
      </div>
    </div>

    <div class="badge b2" bind:this={badge2}>
      <div class="badge-icon">
        <svg viewBox="0 0 24 24"><path d="M12 2v20M17 5H9.5a3.5 3.5 0 0 0 0 7h5a3.5 3.5 0 0 1 0 7H6"/></svg>
      </div>
      <div class="badge-text">
        <p class="badge-val">5-Day Funding</p>
        <p class="badge-lbl">Fast Approvals</p>
      </div>
    </div>

    <div class="badge b3" bind:this={badge3}>
      <div class="badge-icon">
        <svg viewBox="0 0 24 24"><path d="M12 22s8-4 8-10V5l-8-3-8 3v7c0 6 8 10 8 10z"/></svg>
      </div>
      <div class="badge-text">
        <p class="badge-val">20+ Years</p>
        <p class="badge-lbl">Experience</p>
      </div>
    </div>
  </div>

  <!-- Main content -->
  <div class="content">

    <!-- Eyebrow -->
    <p class="eyebrow" bind:this={eyebrowRef}>
      Flexible · Fast · Proven
    </p>

    <!-- Headline -->
    <h1 class="h1" bind:this={h1Ref}>
      Smart Funding<br />
      <span class="h1-accent"><em>Solutions</em></span>
    </h1>

    <!-- Sub -->
    <p class="sub" bind:this={subRef}>
      Funding built around <strong>your goals</strong>, not the bank's rules — with flexible options,
      <strong>faster approvals</strong>, and strategies designed to put you back in control.
    </p>

    <!-- CTAs -->
    <div class="cta-wrap" bind:this={ctaRef}>
      <!-- svelte-ignore event_directive_deprecated -->
      <!-- svelte-ignore a11y_no_static_element_interactions -->
      <div
        onmousemove={onCtaMove}
        onmouseleave={onCtaLeave}
        style="display:inline-block;"
      >
        <a href="/book-call" class="cta-primary" bind:this={ctaInnerRef}>
          <span>Book a Free Call</span>
          <div class="cta-shine"></div>
          <svg viewBox="0 0 24 24"><path d="M5 12h14M12 5l7 7-7 7"/></svg>
        </a>
      </div>

      <a href="/solutions" class="cta-ghost">
        Explore Solutions
        <svg viewBox="0 0 24 24"><path d="M9 18l6-6-6-6"/></svg>
      </a>
    </div>
  </div>

  <!-- Kinetic ticker -->
  <div class="ticker-outer" bind:this={tickerRef}>
    <div class="ticker-inner">
      <div class="ticker-track">
        {#each Array(8) as _}
          <span class="tick-item">
            <span class="tick-dot"></span>
            <span>Smart Funding</span>
            <span class="tick-dot"></span>
            <span class="tick-accent">90+ Lenders</span>
            <span class="tick-dot"></span>
            <span>5-Day Approval</span>
            <span class="tick-dot"></span>
            <span class="tick-accent">20 Years Experience</span>
            <span class="tick-dot"></span>
            <span>Homeowners · Investors · High Net Worth</span>
          </span>
        {/each}
      </div>
    </div>
  </div>

  <!-- Scroll indicator -->
  <div class="scroll-ind" bind:this={scrollEl}>
    <p class="scroll-text">Scroll</p>
    <div class="scroll-pill">
      <div class="pulse-dot"></div>
    </div>
  </div>

</section>