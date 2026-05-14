<script>
  import { onMount } from 'svelte';
  import logo from "$lib/assets/logo.webp"

  let isOpen = $state(false);
  let scrolled = $state(false);
  let menuMounted = $state(false);

  const navLinks = [
    { label: 'Essays', href: '/blog' },
    { label: 'Guides', href: '/guides' },
    { label: 'About', href: '/about' },
    { label: 'Contact', href: '/contact' }
  ];

  function toggleMenu() {
    if (!isOpen) {
      isOpen = true;
      // Small delay so the DOM mounts, then trigger the animation
      requestAnimationFrame(() => {
        requestAnimationFrame(() => { menuMounted = true; });
      });
    } else {
      menuMounted = false;
      // Wait for exit animation before removing from DOM
      setTimeout(() => { isOpen = false; }, 400);
    }
    if (typeof document !== 'undefined') {
      document.body.style.overflow = isOpen ? '' : 'hidden';
    }
  }

  function closeMenu() {
    menuMounted = false;
    setTimeout(() => { isOpen = false; }, 400);
    if (typeof document !== 'undefined') {
      document.body.style.overflow = '';
    }
  }

  onMount(() => {
    const handleScroll = () => scrolled = window.scrollY > 10;
    window.addEventListener('scroll', handleScroll, { passive: true });
    return () => window.removeEventListener('scroll', handleScroll);
  });
</script>

<style>
  /* ── Mobile menu panel ── */
  .mobile-panel {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 100svh;
    background: #0a0a0a;
    padding: 6rem 2rem 2rem;
    overflow: hidden;

    /* Slide + fade in from the top */
    transform: translateY(-12px);
    opacity: 0;
    transition:
      transform 0.4s cubic-bezier(0.16, 1, 0.3, 1),
      opacity   0.35s ease;
  }

  .mobile-panel.open {
    transform: translateY(0);
    opacity: 1;
  }

  /* Staggered nav links */
  .mobile-panel .nav-link {
    display: block;
    border-bottom: 1px solid rgba(255,255,255,0.08);
    padding: 1.25rem 0;
    font-size: clamp(2rem, 8vw, 2.75rem);
    font-family: Georgia, 'Times New Roman', serif;
    font-weight: 500;
    color: rgba(255,255,255,0.35);
    text-decoration: none;
    letter-spacing: -0.02em;
    transform: translateX(-20px);
    opacity: 0;
    transition:
      color     0.2s ease,
      transform 0.45s cubic-bezier(0.16, 1, 0.3, 1),
      opacity   0.35s ease;
  }

  .mobile-panel.open .nav-link:nth-child(1) { transform: translateX(0); opacity: 1; transition-delay: 0.06s; }
  .mobile-panel.open .nav-link:nth-child(2) { transform: translateX(0); opacity: 1; transition-delay: 0.12s; }
  .mobile-panel.open .nav-link:nth-child(3) { transform: translateX(0); opacity: 1; transition-delay: 0.18s; }
  .mobile-panel.open .nav-link:nth-child(4) { transform: translateX(0); opacity: 1; transition-delay: 0.24s; }

  .mobile-panel .nav-link:hover { color: #ffffff; }

  /* Footer actions inside mobile menu */
  .mobile-panel .menu-footer {
    margin-top: 2.5rem;
    display: flex;
    flex-direction: column;
    gap: 0.75rem;
    opacity: 0;
    transform: translateY(8px);
    transition:
      opacity   0.35s ease,
      transform 0.4s cubic-bezier(0.16, 1, 0.3, 1);
    transition-delay: 0s;
  }

  .mobile-panel.open .menu-footer {
    opacity: 1;
    transform: translateY(0);
    transition-delay: 0.32s;
  }

  /* Hamburger button — always visible on any background */
  .hamburger-btn {
    position: relative;
    z-index: 10;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 2.5rem;
    height: 2.5rem;
    /* Pill bg guarantees contrast regardless of page bg */
    background: rgba(255, 255, 255, 0.12);
    border: 1px solid rgba(255, 255, 255, 0.22);
    border-radius: 9999px;
    backdrop-filter: blur(8px);
    -webkit-backdrop-filter: blur(8px);
    cursor: pointer;
    transition:
      background 0.2s ease,
      border-color 0.2s ease;
  }

  .hamburger-btn:hover {
    background: rgba(255, 255, 255, 0.2);
    border-color: rgba(255, 255, 255, 0.35);
  }

  .hamburger-btn:focus-visible {
    outline: 2px solid #fff;
    outline-offset: 3px;
  }

  .bar {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    display: block;
    width: 16px;
    height: 1.5px;
    background: #ffffff;
    border-radius: 2px;
    transition:
      transform 0.35s cubic-bezier(0.16, 1, 0.3, 1),
      opacity   0.25s ease,
      top       0.35s cubic-bezier(0.16, 1, 0.3, 1),
      bottom    0.35s cubic-bezier(0.16, 1, 0.3, 1);
  }

  /* Three bar positions */
  .bar-top    { top: calc(50% - 5px); }
  .bar-mid    { top: 50%; transform: translateX(-50%) translateY(-50%); }
  .bar-bot    { bottom: calc(50% - 5px); }

  /* X state */
  .bar-top.is-open    { top: 50%; transform: translateX(-50%) translateY(-50%) rotate(45deg); }
  .bar-mid.is-open    { opacity: 0; transform: translateX(-50%) translateY(-50%) scaleX(0); }
  .bar-bot.is-open    { bottom: 50%; transform: translateX(-50%) translateY(50%) rotate(-45deg); }
</style>

<header
  class="fixed top-0 left-0 right-0 z-50 border-b transition-all duration-500
  {scrolled || isOpen ? 'border-border bg-background/90 backdrop-blur-xl' : 'border-transparent bg-transparent'}"
>
  <div class="mx-auto flex h-20 max-w-7xl items-center justify-between px-6 lg:px-8">

    <!-- Logo -->
    <a href="/" class="relative z-10 flex items-center gap-3 h-full">
      <span class="flex items-center h-20 justify-center rounded">
        <img src={logo} alt="logo" class="h-full invert grayscale">
      </span>
    </a>

    <!-- Desktop Navigation -->
    <nav class="hidden lg:flex items-center gap-8">
      {#each navLinks as link}
        <a
          href={link.href}
          class="group relative text-sm font-medium text-muted transition-colors hover:text-foreground"
        >
          {link.label}
          <!-- svelte-ignore element_invalid_self_closing_tag -->
          <span class="absolute -bottom-1 left-0 h-px w-0 bg-foreground transition-all duration-300 group-hover:w-full" />
        </a>
      {/each}
    </nav>

    <!-- Desktop Actions -->
    <div class="hidden lg:flex items-center gap-6">
      <a href="/contact" class="text-sm font-medium text-muted hover:text-foreground transition-colors">
        Contact Us
      </a>
    </div>

    <!-- Mobile Hamburger — always-visible pill button -->
    <button
      class="hamburger-btn lg:hidden!"
      onclick={toggleMenu}
      aria-label={isOpen ? 'Close menu' : 'Open menu'}
      aria-expanded={isOpen}
    >
      <span class="bar bar-top {isOpen ? 'is-open' : ''}"></span>
      <span class="bar bar-mid {isOpen ? 'is-open' : ''}"></span>
      <span class="bar bar-bot {isOpen ? 'is-open' : ''}"></span>
    </button>
  </div>

  <!-- Mobile Menu -->
  {#if isOpen}
    <div class="lg:hidden! mobile-panel {menuMounted ? 'open' : ''}">
      <nav>
        {#each navLinks as link}
          <a
            href={link.href}
            onclick={closeMenu}
            class="nav-link"
          >
            {link.label}
          </a>
        {/each}
      </nav>

      <div class="menu-footer">
        <!-- <a
          href="/login"
          onclick={closeMenu}
          class="py-3 text-center text-sm font-medium text-white/50 hover:text-white transition-colors"
        >
          Sign In
        </a> -->
        <a
          href="/contact"
          onclick={closeMenu}
          class="rounded-full bg-white py-4 text-center text-sm font-semibold text-black hover:bg-white/90 transition-colors"
        >
          Contact Us
        </a>
      </div>
    </div>
  {/if}
</header>

<div class="h-18"></div>