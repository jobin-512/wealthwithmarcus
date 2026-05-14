<script>
// @ts-nocheck

  import { onMount } from 'svelte';
  import marcus from "$lib/assets/home/marcus.webp"

  let sectionEl;
  let headingEl;
  let imageWrapEl;
  let imageEl;
  let tagEl;
  let bodyEl;
  let ctaEl;
  let statsEl;
  let overlayEl;
  let glowEl;
  let curtainEl;
  let scratchEl;

  onMount(async () => {
    const { gsap } = await import('gsap');
    const { ScrollTrigger } = await import('gsap/ScrollTrigger');
    const { SplitText } = await import('gsap/SplitText');
    gsap.registerPlugin(ScrollTrigger, SplitText);

    // ── Split heading into chars ──────────────────────────────────────
    const split = new SplitText(headingEl, { type: 'lines,words,chars' });
    const chars = split.chars;

    // ── Master timeline ───────────────────────────────────────────────
    const tl = gsap.timeline({
      scrollTrigger: {
        trigger: sectionEl,
        start: 'top 75%',
        once: true,
      }
    });

    // 1. Curtain wipe on image (left-to-right reveal)
    tl.fromTo(curtainEl,
      { scaleX: 1, transformOrigin: 'left center' },
      { scaleX: 0, duration: 1.1, ease: 'expo.inOut' },
      0
    );

    // 2. Image scale-in from slightly zoomed
    tl.fromTo(imageEl,
      { scale: 1.18 },
      { scale: 1, duration: 1.4, ease: 'expo.out' },
      0.1
    );

    // 3. Label tag slides in
    tl.fromTo(tagEl,
      { y: 20, opacity: 0 },
      { y: 0, opacity: 1, duration: 0.5, ease: 'power3.out' },
      0.35
    );

    // 4. Chars rain down one by one
    tl.fromTo(chars,
      { y: '110%', opacity: 0, rotateX: -40 },
      {
        y: '0%',
        opacity: 1,
        rotateX: 0,
        duration: 0.65,
        stagger: { amount: 0.55 },
        ease: 'back.out(1.4)'
      },
      0.5
    );

    // 5. Body text slides up
    tl.fromTo(bodyEl,
      { y: 30, opacity: 0 },
      { y: 0, opacity: 1, duration: 0.7, ease: 'power3.out' },
      1.0
    );

    // 6. CTA pops in with bounce
    tl.fromTo(ctaEl,
      { scale: 0.8, opacity: 0 },
      { scale: 1, opacity: 1, duration: 0.6, ease: 'back.out(2)' },
      1.25
    );

    // 7. Stats slide in
    tl.fromTo(statsEl.children,
      { y: 40, opacity: 0 },
      { y: 0, opacity: 1, duration: 0.55, stagger: 0.12, ease: 'power3.out' },
      1.15
    );

    // ── Parallax: image drifts up on scroll ───────────────────────────
    gsap.to(imageEl, {
      y: -80,
      ease: 'none',
      scrollTrigger: {
        trigger: sectionEl,
        start: 'top bottom',
        end: 'bottom top',
        scrub: 1.5,
      }
    });

    // ── Parallax: glow orb moves opposite direction ───────────────────
    gsap.to(glowEl, {
      y: 60,
      x: -30,
      ease: 'none',
      scrollTrigger: {
        trigger: sectionEl,
        start: 'top bottom',
        end: 'bottom top',
        scrub: 2,
      }
    });

    // ── Magnetic CTA button ───────────────────────────────────────────
    const btn = ctaEl.querySelector('a');
    if (btn) {
      btn.addEventListener('mousemove', (e) => {
        const rect = btn.getBoundingClientRect();
        const cx = rect.left + rect.width / 2;
        const cy = rect.top + rect.height / 2;
        const dx = (e.clientX - cx) * 0.35;
        const dy = (e.clientY - cy) * 0.35;
        gsap.to(btn, { x: dx, y: dy, duration: 0.4, ease: 'power2.out' });
      });
      btn.addEventListener('mouseleave', () => {
        gsap.to(btn, { x: 0, y: 0, duration: 0.6, ease: 'elastic.out(1, 0.5)' });
      });
    }

    // ── Gold line draw on heading hover ──────────────────────────────
    headingEl.addEventListener('mouseenter', () => {
      gsap.fromTo(scratchEl,
        { scaleX: 0, transformOrigin: 'left center' },
        { scaleX: 1, duration: 0.6, ease: 'expo.out' }
      );
    });
    headingEl.addEventListener('mouseleave', () => {
      gsap.to(scratchEl, { scaleX: 0, transformOrigin: 'right center', duration: 0.4, ease: 'expo.in' });
    });

    // ── Image tilt on mousemove (the whole wrapper) ───────────────────
    imageWrapEl.addEventListener('mousemove', (e) => {
      const rect = imageWrapEl.getBoundingClientRect();
      const x = (e.clientX - rect.left) / rect.width  - 0.5;
      const y = (e.clientY - rect.top)  / rect.height - 0.5;
      gsap.to(imageWrapEl, {
        rotateY: x * 14,
        rotateX: -y * 10,
        scale: 1.025,
        duration: 0.55,
        ease: 'power2.out',
        transformPerspective: 900,
      });
      // shimmer overlay moves with mouse
      gsap.to(overlayEl, {
        opacity: 0.12,
        x: x * 30,
        y: y * 20,
        duration: 0.4,
      });
    });
    imageWrapEl.addEventListener('mouseleave', () => {
      gsap.to(imageWrapEl, { rotateY: 0, rotateX: 0, scale: 1, duration: 0.8, ease: 'elastic.out(1, 0.55)' });
      gsap.to(overlayEl, { opacity: 0, x: 0, y: 0, duration: 0.5 });
    });

    // ── Number count-up for stats ─────────────────────────────────────
    document.querySelectorAll('[data-count]').forEach(el => {
      const target = +el.dataset.count;
      gsap.fromTo({ val: 0 }, { val: target }, {
        val: target,
        duration: 1.8,
        ease: 'power2.out',
        delay: 1.3,
        scrollTrigger: { trigger: sectionEl, start: 'top 75%', once: true },
        onUpdate() { el.textContent = Math.round(this.targets()[0].val) + (el.dataset.suffix || ''); }
      });
    });

    return () => {
      ScrollTrigger.getAll().forEach(t => t.kill());
    };
  });
</script>

<style>
  section {
    position: relative;
    overflow: hidden;
    background: var(--color-surface, #080808);
  }

  /* ── Grain overlay ── */
  section::before {
    content: '';
    position: absolute;
    inset: 0;
    background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.9' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)' opacity='1'/%3E%3C/svg%3E");
    opacity: 0.03;
    pointer-events: none;
    z-index: 0;
  }

  /* ── Ambient glow ── */
  .glow {
    position: absolute;
    right: -5%;
    top: 30%;
    width: 700px;
    height: 700px;
    border-radius: 50%;
    background: radial-gradient(circle, rgba(197,160,89,0.18) 0%, transparent 70%);
    filter: blur(80px);
    pointer-events: none;
  }

  /* ── Decorative corner lines ── */
  .corner-tl, .corner-br {
    position: absolute;
    width: 60px;
    height: 60px;
    pointer-events: none;
  }
  .corner-tl { top: 2rem; left: 2rem; border-top: 1px solid rgba(197,160,89,0.3); border-left: 1px solid rgba(197,160,89,0.3); }
  .corner-br { bottom: 2rem; right: 2rem; border-bottom: 1px solid rgba(197,160,89,0.3); border-right: 1px solid rgba(197,160,89,0.3); }

  /* ── Image wrapper 3-D tilt ── */
  .img-wrap {
    position: relative;
    transform-style: preserve-3d;
    will-change: transform;
    cursor: default;
  }

  /* ── Curtain element ── */
  .curtain {
    position: absolute;
    inset: 0;
    background: var(--color-surface, #080808);
    z-index: 10;
    transform-origin: left center;
    border-radius: inherit;
  }

  /* ── Shimmer overlay ── */
  .shimmer {
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, rgba(197,160,89,0.5), transparent 60%);
    opacity: 0;
    pointer-events: none;
    border-radius: inherit;
    z-index: 5;
    mix-blend-mode: screen;
  }

  /* ── Gold scratch line ── */
  .scratch {
    display: block;
    height: 2px;
    background: linear-gradient(90deg, var(--accent, #c5a059), transparent);
    transform: scaleX(0);
    transform-origin: left center;
    margin-top: 0.5rem;
    border-radius: 2px;
  }

  /* ── Stat cards ── */
  .stat-card {
    padding: 1.25rem 1.5rem;
    border: 1px solid rgba(197,160,89,0.15);
    border-radius: 0.75rem;
    background: rgba(197,160,89,0.04);
    backdrop-filter: blur(8px);
    position: relative;
    overflow: hidden;
  }
  .stat-card::before {
    content: '';
    position: absolute;
    inset: 0;
    background: linear-gradient(135deg, rgba(197,160,89,0.08), transparent);
    pointer-events: none;
  }
  .stat-num {
    font-family: Georgia, 'Times New Roman', serif;
    font-size: clamp(2rem, 4vw, 2.75rem);
    font-weight: 400;
    color: var(--accent, #c5a059);
    line-height: 1;
    letter-spacing: -0.03em;
  }
  .stat-label {
    font-size: 0.7rem;
    text-transform: uppercase;
    letter-spacing: 0.18em;
    color: rgba(255,255,255,0.35);
    margin-top: 0.4rem;
  }

  /* ── Overflow clip for char animation ── */
  :global(.split-line) { overflow: hidden; }
</style>

<section bind:this={sectionEl}>
  <!-- Grain + corners handled via CSS; glow is DOM -->
  <div class="glow" bind:this={glowEl}></div>
  <div class="corner-tl"></div>
  <div class="corner-br"></div>

  <div class="relative mx-auto max-w-7xl px-6 py-28 lg:py-36 z-10">
    <div class="grid grid-cols-1 items-center gap-16 lg:grid-cols-2 lg:gap-28">

      <!-- ── Text Content ── -->
      <div>
        <!-- Label -->
        <p
          bind:this={tagEl}
          class="mb-6 font-mono text-xs uppercase tracking-[0.25em] text-muted opacity-0"
          style="color: rgba(197,160,89,0.7);"
        >
          ✦ About
        </p>

        <!-- Heading with char split -->
        <div bind:this={headingEl} class="relative cursor-default">
          <h2
            class="font-serif text-5xl font-medium leading-[1.08] tracking-tight sm:text-6xl lg:text-7xl"
            style="color: #f5f0e8; perspective: 600px;"
          >
            Meet Marcus:<br />
            Your Trusted<br />
            <em style="color: var(--accent, #c5a059); font-style: italic;">Mortgage Expert</em>
          </h2>
          <span class="scratch" bind:this={scratchEl}></span>
        </div>

        <!-- Body -->
        <p
          bind:this={bodyEl}
          class="mt-8 max-w-lg text-base leading-relaxed sm:text-lg opacity-0"
          style="color: rgba(255,255,255,0.5);"
        >
          With over 20+ years in the finance industry, I pride myself on providing clear,
          customized mortgage solutions. My approachable style ensures that you feel
          comfortable and informed every step of the way.
        </p>

        <!-- Stats row -->
        <div
          bind:this={statsEl}
          class="mt-10 grid grid-cols-3 gap-3"
        >
          <div class="stat-card">
            <div class="stat-num"><span data-count="20" data-suffix="+">0+</span></div>
            <div class="stat-label">Years Experience</div>
          </div>
          <div class="stat-card">
            <div class="stat-num"><span data-count="850" data-suffix="+">0+</span></div>
            <div class="stat-label">Loans Closed</div>
          </div>
          <div class="stat-card">
            <div class="stat-num"><span data-count="98" data-suffix="%">0%</span></div>
            <div class="stat-label">Satisfaction</div>
          </div>
        </div>

        <!-- CTA -->
        <div class="mt-10" bind:this={ctaEl} style="opacity:0;">
          <a
            href="/contact"
            class="inline-flex items-center gap-3 rounded-full px-8 py-4 text-sm font-semibold transition-colors"
            style="background: var(--accent, #c5a059); color: #080808;"
          >
            Get Started Today
            <svg width="16" height="16" viewBox="0 0 16 16" fill="none" aria-hidden="true">
              <path d="M3 8h10M9 4l4 4-4 4" stroke="currentColor" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
            </svg>
          </a>
        </div>
      </div>

      <!-- ── Image ── -->
      <div class="relative mx-auto w-full max-w-md lg:max-w-none">
        <div class="img-wrap" bind:this={imageWrapEl}>

          <!-- Gold rule accent -->
          <div style="
            position: absolute;
            top: -1.5rem; left: -1.5rem;
            width: 60%; height: 60%;
            border-top: 1px solid rgba(197,160,89,0.25);
            border-left: 1px solid rgba(197,160,89,0.25);
            border-radius: 0.5rem 0 0 0;
            pointer-events: none;
            z-index: 1;
          "></div>

          <!-- Image container -->
          <div
            class="relative overflow-hidden rounded-2xl"
            style="aspect-ratio: 3/4; background: #111;"
          >
            <img
              bind:this={imageEl}
              src={marcus}
              alt="Marcus – Your Trusted Mortgage Expert"
              class="h-full w-full object-cover"
              style="will-change: transform;"
            />
            <!-- Bottom dark fade -->
            <div style="
              position: absolute; inset: 0;
              background: linear-gradient(to top, rgba(8,8,8,0.55) 0%, transparent 45%);
              z-index: 2;
              pointer-events: none;
            "></div>

            <!-- Shimmer layer for mouse tilt -->
            <div class="shimmer" bind:this={overlayEl}></div>

            <!-- Curtain reveal -->
            <div class="curtain" bind:this={curtainEl}></div>
          </div>

          <!-- Bottom-right accent rule -->
          <div style="
            position: absolute;
            bottom: -1.5rem; right: -1.5rem;
            width: 45%; height: 45%;
            border-bottom: 1px solid rgba(197,160,89,0.25);
            border-right: 1px solid rgba(197,160,89,0.25);
            border-radius: 0 0 0.5rem 0;
            pointer-events: none;
            z-index: 1;
          "></div>

          <!-- Floating name badge -->
          <div style="
            position: absolute;
            bottom: 1.5rem; left: -1rem;
            background: rgba(8,8,8,0.85);
            border: 1px solid rgba(197,160,89,0.25);
            border-radius: 0.75rem;
            padding: 0.75rem 1.25rem;
            backdrop-filter: blur(12px);
            z-index: 6;
          ">
            <p style="font-family: Georgia, serif; font-size: 1rem; color: #f5f0e8; margin: 0; line-height: 1.2;">Marcus Montrose</p>
            <p style="font-size: 0.65rem; text-transform: uppercase; letter-spacing: 0.18em; color: rgba(197,160,89,0.7); margin: 0.25rem 0 0;">Senior Mortgage Advisor</p>
          </div>
        </div>
      </div>

    </div>
  </div>
</section>