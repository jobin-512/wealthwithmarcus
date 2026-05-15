<script>
// @ts-nocheck

  import { onMount } from 'svelte';
  import logo from "$lib/assets/logo.webp"

  let isOpen = $state(false);
  let menuMounted = $state(false);
  let scrolled = $state(false);
  let activeDropdown = $state(null);
  let mobileExpanded = $state(null);
  let dropdownTimer;

  const solutions = [
    {
      label: 'Homeowners',
      href: '/solutions/homeowners',
      desc: 'Purchase, refinance & equity strategies',
      icon: 'M3 9.75L12 3l9 6.75V21a1 1 0 01-1 1H4a1 1 0 01-1-1V9.75z M9 22V12h6v10'
    },
    {
      label: 'Investors',
      href: '/solutions/investors',
      desc: 'Portfolio growth & DSCR lending',
      icon: 'M2.25 18L9 11.25l4.306 4.307a11.95 11.95 0 015.814-5.519l2.74-1.22m0 0l-5.94-2.28m5.94 2.28l-2.28 5.941'
    },
    {
      label: 'High Net Worth',
      href: '/solutions/high-net-worth',
      desc: 'Jumbo, private & bespoke structures',
      icon: 'M12 6v12m-3-2.818l.879.659c1.171.879 3.07.879 4.242 0 1.172-.879 1.172-2.303 0-3.182C13.536 12.219 12.768 12 12 12c-.725 0-1.45-.22-2.003-.659-1.106-.879-1.106-2.303 0-3.182s2.9-.879 4.006 0l.415.33M21 12a9 9 0 11-18 0 9 9 0 0118 0z'
    }
  ];

  const navLinks = [
    { label: 'Home',            href: '/',                dropdown: null },
    { label: 'Solutions',       href: '/solutions',       dropdown: 'solutions' },
    { label: 'heloc',        href: '/heloc',        dropdown: null },
    { label: 'Strategy',        href: '/strategy',        dropdown: null },
    { label: 'Results',         href: '/results',         dropdown: null },
    { label: 'Learning Center', href: '/learning-center', dropdown: null },
  ];

  function openDrop(key) { clearTimeout(dropdownTimer); activeDropdown = key; }
  function closeDrop()   { dropdownTimer = setTimeout(() => { activeDropdown = null; }, 130); }
  function stayOpen()    { clearTimeout(dropdownTimer); }

  function toggleMenu() {
    if (!isOpen) {
      isOpen = true;
      requestAnimationFrame(() => requestAnimationFrame(() => { menuMounted = true; }));
      if (typeof document !== 'undefined') document.body.style.overflow = 'hidden';
    } else {
      menuMounted = false;
      setTimeout(() => { isOpen = false; mobileExpanded = null; }, 400);
      if (typeof document !== 'undefined') document.body.style.overflow = '';
    }
  }

  function closeMenu() {
    menuMounted = false;
    setTimeout(() => { isOpen = false; mobileExpanded = null; }, 400);
    if (typeof document !== 'undefined') document.body.style.overflow = '';
  }

  function toggleAccordion(key) {
    mobileExpanded = mobileExpanded === key ? null : key;
  }

  onMount(() => {
    const fn = () => { scrolled = window.scrollY > 10; };
    window.addEventListener('scroll', fn, { passive: true });
    return () => window.removeEventListener('scroll', fn);
  });
</script>

<!-- svelte-ignore css_unused_selector -->
<style>
/* ════════════════════════════════
   BASE
════════════════════════════════ */
header {
  position: fixed; top: 0; left: 0; right: 0; z-index: 50;
  border-bottom: 1px solid transparent;
  transition: border-color .45s, background .45s, backdrop-filter .45s;
}
header.scrolled {
  border-color: rgba(255,255,255,.07);
  background: rgba(6,6,10,.88);
  backdrop-filter: blur(22px); -webkit-backdrop-filter: blur(22px);
}
header.open {
  border-color: rgba(255,255,255,.07);
  background: rgba(6,6,10,.96);
}

.inner {
  max-width: 88rem; margin: 0 auto;
  display: flex; align-items: center; justify-content: space-between;
  height: 4.75rem; padding: 0 1.75rem;
}

/* ════════════════════════════════
   LOGO
════════════════════════════════ */
.logo {
  display: flex; align-items: center; height: 100%;
  position: relative; z-index: 10; text-decoration: none;
}
.logo img { height: 3rem; filter: invert(1) grayscale(1); }

/* ════════════════════════════════
   DESKTOP NAV
════════════════════════════════ */
.d-nav {
  display: none; align-items: center; gap: 0;
}
@media (min-width: 1024px) { .d-nav { display: flex; } }

.nav-item { position: relative; }

.nav-a {
  display: inline-flex; align-items: center; gap: .3rem;
  padding: .5rem .5rem .5rem .9rem;
  font-size: .8rem; font-weight: 500; letter-spacing: .025em;
  color: rgba(255,255,255,.5);
  text-decoration: none;
  white-space: nowrap; position: relative;
  transition: color .2s;
}
.nav-a:hover, .nav-a.act { color: rgba(255,255,255,.92); }

/* underline */
.nav-a::after {
  content: ''; position: absolute;
  bottom: 4px; left: .9rem; right: .5rem;
  height: 1px; background: rgba(197,160,89,.75);
  transform: scaleX(0); transform-origin: left;
  transition: transform .25s ease;
}
.nav-a:hover::after, .nav-a.act::after { transform: scaleX(1); }

.nav-drop-trigger {
  display: flex; align-items: center; gap: 0;
}

.chev-btn {
  display: flex; align-items: center; justify-content: center;
  padding: .5rem .4rem .5rem .1rem;
  background: none; border: none; cursor: pointer;
  color: rgba(255,255,255,.5);
  transition: color .2s;
}
.chev-btn:hover, .nav-drop-trigger:hover .chev-btn { color: rgba(255,255,255,.92); }

.chev {
  width: 11px; height: 11px; flex-shrink: 0;
  stroke: currentColor; fill: none; stroke-width: 2.2;
  stroke-linecap: round; stroke-linejoin: round;
  transition: transform .3s cubic-bezier(.16,1,.3,1);
}
.chev.up { transform: rotate(180deg); }

/* ════════════════════════════════
   DROPDOWN
════════════════════════════════ */
.drop {
  position: absolute; top: calc(100% + .9rem); left: 50%;
  width: 38rem;
  background: rgba(9,9,13,.97);
  border: 1px solid rgba(255,255,255,.08);
  border-radius: 1.15rem;
  padding: 1.35rem;
  backdrop-filter: blur(28px); -webkit-backdrop-filter: blur(28px);
  box-shadow: 0 40px 100px -20px rgba(0,0,0,.75), 0 0 0 1px rgba(197,160,89,.05);
  opacity: 0; pointer-events: none;
  transform: translateX(-50%) translateY(-10px);
  transition: opacity .22s ease, transform .3s cubic-bezier(.16,1,.3,1);
  z-index: 60;
}
.drop.vis {
  opacity: 1; pointer-events: all;
  transform: translateX(-50%) translateY(0);
}
.drop::before {
  content: ''; position: absolute; top: -5px; left: 50%;
  width: 10px; height: 10px;
  background: rgba(9,9,13,.97);
  border-left: 1px solid rgba(255,255,255,.08);
  border-top:  1px solid rgba(255,255,255,.08);
  transform: translateX(-50%) rotate(45deg);
}

.drop-lbl {
  font-family: 'Courier New', monospace; font-size: .6rem;
  text-transform: uppercase; letter-spacing: .22em;
  color: rgba(197,160,89,.48);
  padding-bottom: .9rem;
  border-bottom: 1px solid rgba(255,255,255,.05);
  margin-bottom: .9rem;
}

.drop-grid { display: grid; grid-template-columns: repeat(3,1fr); gap: .55rem; }

.drop-card {
  display: block; text-decoration: none;
  padding: 1rem .9rem; border-radius: .8rem;
  border: 1px solid transparent;
  transition: background .2s, border-color .2s, transform .28s cubic-bezier(.16,1,.3,1);
}
.drop-card:hover {
  background: rgba(197,160,89,.055);
  border-color: rgba(197,160,89,.16);
  transform: translateY(-2px);
}
.di {
  width: 2.2rem; height: 2.2rem; border-radius: .5rem;
  border: 1px solid rgba(197,160,89,.2);
  background: rgba(197,160,89,.07);
  display: flex; align-items: center; justify-content: center;
  margin-bottom: .8rem;
  transition: border-color .2s, background .2s;
}
.drop-card:hover .di {
  border-color: rgba(197,160,89,.4);
  background: rgba(197,160,89,.12);
}
.di svg { width: 13px; height: 13px; stroke: rgba(197,160,89,.8); fill: none; stroke-width: 1.8; stroke-linecap: round; stroke-linejoin: round; }
.dt { font-size: .8rem; font-weight: 600; color: rgba(255,255,255,.85); margin-bottom: .2rem; }
.dd { font-size: .7rem; line-height: 1.55; color: rgba(255,255,255,.33); }

/* ════════════════════════════════
   DESKTOP CTA
════════════════════════════════ */
.d-cta { display: none; align-items: center; gap: .75rem; }
@media (min-width: 1024px) { .d-cta { display: flex; } }

.ghost {
  font-size: .78rem; font-weight: 500; letter-spacing: .03em;
  color: rgba(255,255,255,.45); text-decoration: none;
  padding: .45rem .7rem; transition: color .2s;
}
.ghost:hover { color: rgba(255,255,255,.9); }

.solid {
  display: inline-flex; align-items: center; gap: .45rem;
  font-size: .75rem; font-weight: 700;
  letter-spacing: .09em; text-transform: uppercase;
  color: #06060a;
  background: linear-gradient(135deg,#c5a059,#9a7430);
  padding: .6rem 1.4rem; border-radius: 999px;
  text-decoration: none; position: relative; overflow: hidden;
  transition: box-shadow .3s, transform .3s cubic-bezier(.23,1,.32,1);
}
.solid:hover { box-shadow: 0 0 55px -8px rgba(197,160,89,.6); transform: translateY(-1px); }
.solid-shine {
  position: absolute; inset: 0;
  background: linear-gradient(105deg,transparent 35%,rgba(255,255,255,.22) 50%,transparent 65%);
  transform: translateX(-100%); transition: transform .7s ease;
}
.solid:hover .solid-shine { transform: translateX(100%); }
.solid svg { width: 13px; height: 13px; stroke: currentColor; fill: none; stroke-width: 2.5; stroke-linecap: round; stroke-linejoin: round; }

/* ════════════════════════════════
   HAMBURGER
════════════════════════════════ */
.ham {
  display: flex; align-items: center; justify-content: center;
  width: 2.5rem; height: 2.5rem; border-radius: 999px;
  background: rgba(255,255,255,.1);
  border: 1px solid rgba(255,255,255,.2);
  backdrop-filter: blur(8px); -webkit-backdrop-filter: blur(8px);
  cursor: pointer; z-index: 10; position: relative;
  transition: background .2s, border-color .2s;
}
.ham:hover { background: rgba(255,255,255,.18); border-color: rgba(255,255,255,.32); }
@media (min-width: 1024px) { .ham { display: none; } }

.b {
  position: absolute; left: 50%; display: block;
  width: 16px; height: 1.5px; background: #fff; border-radius: 2px;
  transition: transform .35s cubic-bezier(.16,1,.3,1),
              opacity   .25s ease,
              top       .35s cubic-bezier(.16,1,.3,1),
              bottom    .35s cubic-bezier(.16,1,.3,1);
}
.bt { top: calc(50% - 5px); transform: translateX(-50%); }
.bm { top: 50%; transform: translateX(-50%) translateY(-50%); }
.bb { bottom: calc(50% - 5px); transform: translateX(-50%); }
.bt.o { top: 50%;    transform: translateX(-50%) translateY(-50%) rotate(45deg); }
.bm.o { opacity: 0;  transform: translateX(-50%) translateY(-50%) scaleX(0); }
.bb.o { bottom: 50%; transform: translateX(-50%) translateY(50%) rotate(-45deg); }

/* ════════════════════════════════
   MOBILE PANEL
════════════════════════════════ */
.mpanel {
  position: fixed; top: 0; left: 0; right: 0; bottom: 0;
  height: 100svh;
  background: #06060a;
  padding: 5.5rem 1.75rem 2.5rem;
  overflow-y: auto; z-index: 49;
  opacity: 0; transform: translateY(-10px);
  transition: opacity .38s ease, transform .4s cubic-bezier(.16,1,.3,1);
}
.mpanel.on { opacity: 1; transform: translateY(0); }

.mpanel::before {
  content: ''; display: block; height: 1px; margin-bottom: 2rem;
  background: linear-gradient(90deg,transparent,rgba(197,160,89,.28),transparent);
}

/* row — use a data attr to avoid ::before pseudo-element nth-child offset */
.mrow {
  border-bottom: 1px solid rgba(255,255,255,.055);
  opacity: 0; transform: translateX(-16px);
  transition: opacity .42s cubic-bezier(.16,1,.3,1), transform .45s cubic-bezier(.16,1,.3,1);
}
.mrow:nth-of-type(1){ transition-delay:.04s; }
.mrow:nth-of-type(2){ transition-delay:.09s; }
.mrow:nth-of-type(3){ transition-delay:.14s; }
.mrow:nth-of-type(4){ transition-delay:.19s; }
.mrow:nth-of-type(5){ transition-delay:.24s; }
.mpanel.on .mrow { opacity:1; transform:none; }

/* link / button */
.ml {
  display: flex; align-items: center; justify-content: space-between;
  padding: 1.1rem 0;
  font-family: Georgia,'Times New Roman',serif;
  font-size: clamp(1.7rem,7vw,2.4rem);
  font-weight: 500; letter-spacing: -.025em;
  color: rgba(255,255,255,.28);
  text-decoration: none; background: none; border: none;
  cursor: pointer; width: 100%; text-align: left;
  transition: color .2s;
}
.ml:hover, .ml.act { color: rgba(255,255,255,.9); }

/* Solutions row: link + chevron button side by side */
.ml-row {
  display: flex; align-items: center;
  border: none; background: none;
}
.flex-1 { flex: 1; }

.ml-chev {
  display: flex; align-items: center; justify-content: center;
  padding: 1.1rem .5rem;
  background: none; border: none; cursor: pointer;
  color: rgba(255,255,255,.28);
  transition: color .2s;
  flex-shrink: 0;
}
.ml-chev:hover, .ml-chev.act { color: rgba(255,255,255,.9); }

.mc { /* mobile chevron */
  width: 20px; height: 20px; flex-shrink: 0;
  stroke: currentColor; fill: none; stroke-width: 1.8;
  stroke-linecap: round; stroke-linejoin: round;
  transition: transform .35s cubic-bezier(.16,1,.3,1);
}
.mc.r { transform: rotate(180deg); }

/* accordion sub */
.msub { overflow: hidden; max-height: 0; transition: max-height .42s cubic-bezier(.16,1,.3,1); }
.msub.ex { max-height: 420px; }
.msub-in { padding: .4rem 0 1rem .75rem; display: flex; flex-direction: column; gap: .3rem; }

.msl {
  display: flex; align-items: center; gap: .7rem;
  padding: .7rem .85rem; border-radius: .75rem;
  text-decoration: none;
  transition: background .2s;
}
.msl:hover { background: rgba(197,160,89,.06); }

.msi {
  width: 2rem; height: 2rem; border-radius: .45rem; flex-shrink: 0;
  border: 1px solid rgba(197,160,89,.2); background: rgba(197,160,89,.06);
  display: flex; align-items: center; justify-content: center;
}
.msi svg { width: 12px; height: 12px; stroke: rgba(197,160,89,.8); fill: none; stroke-width: 1.8; stroke-linecap: round; stroke-linejoin: round; }
.msl-title { font-size: .88rem; font-weight: 600; color: rgba(255,255,255,.75); }
.msl-desc  { font-size: .7rem; color: rgba(255,255,255,.3); margin-top: .08rem; }

/* mobile footer */
.mfooter {
  margin-top: 2.5rem; display: flex; flex-direction: column; gap: .75rem;
  opacity: 0; transform: translateY(8px);
  transition: opacity .35s, transform .4s cubic-bezier(.16,1,.3,1);
}
.mpanel.on .mfooter { opacity: 1; transform: translateY(0); transition-delay: .3s; }

.mcta {
  display: flex; align-items: center; justify-content: center; gap: .5rem;
  padding: 1rem; border-radius: 999px;
  font-size: .82rem; font-weight: 700; letter-spacing: .07em; text-transform: uppercase;
  background: linear-gradient(135deg,#c5a059,#9a7430);
  color: #06060a; text-decoration: none;
  transition: box-shadow .3s;
}
.mcta:hover { box-shadow: 0 0 50px -8px rgba(197,160,89,.5); }
.mcta svg { width: 14px; height: 14px; stroke: currentColor; fill: none; stroke-width: 2.5; stroke-linecap: round; stroke-linejoin: round; }
</style>

<header class:scrolled class:open={isOpen}>
  <div class="inner">

    <!-- Logo -->
    <a href="/" class="logo">
      <img src={logo} alt="Marcus Mitchell Mortgage" />
    </a>

    <!-- Desktop nav -->
    <nav class="d-nav" aria-label="Main navigation">
      {#each navLinks as link}
        <!-- svelte-ignore a11y_no_static_element_interactions -->
        <div
          class="nav-item"
          onmouseenter={() => link.dropdown ? openDrop(link.dropdown) : null}
          onmouseleave={() => link.dropdown ? closeDrop() : null}
        >
          {#if link.dropdown}
            <div class="nav-drop-trigger">
              <a href={link.href} class="nav-a {activeDropdown === link.dropdown ? 'act' : ''}">{link.label}</a>
              <button class="chev-btn" aria-haspopup="true" aria-expanded={activeDropdown === link.dropdown} aria-label="Open submenu">
                <svg class="chev {activeDropdown === link.dropdown ? 'up' : ''}" viewBox="0 0 24 24">
                  <path d="M6 9l6 6 6-6"/>
                </svg>
              </button>
            </div>

            {#if link.dropdown === 'solutions'}
              <!-- svelte-ignore a11y_interactive_supports_focus -->
              <div
                class="drop {activeDropdown === 'solutions' ? 'vis' : ''}"
                onmouseenter={stayOpen}
                onmouseleave={closeDrop}
                role="menu"
              >
                <p class="drop-lbl">Choose your path</p>
                <div class="drop-grid">
                  {#each solutions as s}
                    <a href={s.href} class="drop-card" role="menuitem">
                      <div class="di"><svg viewBox="0 0 24 24"><path d={s.icon}/></svg></div>
                      <p class="dt">{s.label}</p>
                      <p class="dd">{s.desc}</p>
                    </a>
                  {/each}
                </div>
              </div>
            {/if}
          {:else}
            <a href={link.href} class="nav-a">{link.label}</a>
          {/if}
        </div>
      {/each}
    </nav>

    <!-- Desktop actions -->
    <div class="d-cta">
      <a href="/contact" class="ghost">Contact</a>
      <a href="/book-call" class="solid">
        Book a Call
        <div class="solid-shine"></div>
        <svg viewBox="0 0 24 24"><path d="M5 12h14M12 5l7 7-7 7"/></svg>
      </a>
    </div>

    <!-- Hamburger -->
    <button class="ham" onclick={toggleMenu} aria-label={isOpen ? 'Close menu' : 'Open menu'} aria-expanded={isOpen}>
      <span class="b bt {isOpen ? 'o' : ''}"></span>
      <span class="b bm {isOpen ? 'o' : ''}"></span>
      <span class="b bb {isOpen ? 'o' : ''}"></span>
    </button>

  </div>

  <!-- Mobile panel -->
  {#if isOpen}
    <div class="mpanel {menuMounted ? 'on' : ''}" role="dialog" aria-modal="true" aria-label="Navigation menu">

      <!-- Home -->
      <div class="mrow">
        <a href="/" class="ml" onclick={closeMenu}>Home</a>
      </div>

      <!-- Solutions accordion -->
      <div class="mrow">
        <div class="ml-row">
          <a href="/solutions" class="ml flex-1" onclick={closeMenu}>Solutions</a>
          <button
            class="ml-chev {mobileExpanded === 'solutions' ? 'act' : ''}"
            onclick={() => toggleAccordion('solutions')}
            aria-expanded={mobileExpanded === 'solutions'}
            aria-label="Toggle Solutions submenu"
          >
            <svg class="mc {mobileExpanded === 'solutions' ? 'r' : ''}" viewBox="0 0 24 24">
              <path d="M6 9l6 6 6-6"/>
            </svg>
          </button>
        </div>
        <div class="msub {mobileExpanded === 'solutions' ? 'ex' : ''}">
          <div class="msub-in">
            {#each solutions as s}
              <a href={s.href} class="msl" onclick={closeMenu}>
                <div class="msi"><svg viewBox="0 0 24 24"><path d={s.icon}/></svg></div>
                <div>
                  <p class="msl-title">{s.label}</p>
                  <p class="msl-desc">{s.desc}</p>
                </div>
              </a>
            {/each}
          </div>
        </div>
      </div>

      <!-- Strategy -->
      <div class="mrow">
        <a href="/strategy" class="ml" onclick={closeMenu}>Strategy</a>
      </div>

      <!-- Results -->
      <div class="mrow">
        <a href="/results" class="ml" onclick={closeMenu}>Results</a>
      </div>

      <!-- Learning Center -->
      <div class="mrow">
        <a href="/learning-center" class="ml" onclick={closeMenu}>Learning Center</a>
      </div>

      <!-- Footer CTA -->
      <div class="mfooter">
        <a href="/book-call" class="mcta" onclick={closeMenu}>
          Book a Call
          <svg viewBox="0 0 24 24"><path d="M5 12h14M12 5l7 7-7 7"/></svg>
        </a>
      </div>

    </div>
  {/if}
</header>

<div style="height: 4.75rem;"></div>