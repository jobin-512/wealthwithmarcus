<script>
  import { onMount } from 'svelte';
  import crystal from "$lib/assets/home/letter-crystal-hd.mp4"
  
  let mounted = $state(false);
  onMount(() => (mounted = true));
</script>

<section class="relative flex min-h-[90vh] flex-col items-center justify-center overflow-hidden pt-32 pb-20">

  <!-- svelte-ignore a11y_media_has_caption -->
  <!-- svelte-ignore element_invalid_self_closing_tag -->
  <video src={crystal}  class="h-full w-full object-cover opacity-50 absolute" autoplay loop muted />

  <div class="relative z-10 mx-auto max-w-5xl px-6 text-center">
    {#if mounted}
      <div class="animate-fade-up">
        <!-- Eyebrow -->
        <p class="mb-8 font-mono text-xs uppercase tracking-[0.25em] text-muted">
          <!-- EDIT: Replace with your tagline -->
          Flexible. Fast. Proven.
        </p>

        <!-- 
          Main Headline 
          Use a single impactful phrase. Letter.co uses two words: "Beyond banking"
        -->
        <h1 class="mx-auto max-w-4xl font-serif text-6xl font-medium leading-[0.95] tracking-tight text-foreground sm:text-7xl md:text-8xl lg:text-[7rem]">
          <!-- EDIT: Your headline -->
          Smart Funding <br class="hidden sm:block" />
          <span class="relative inline-block">
            <span class="relative z-10 italic text-accent">Solutions</span>
            <span class="absolute -bottom-1 left-0 h-[0.06em] w-full bg-accent/20 sm:-bottom-2"></span>
          </span>
        </h1>

        <!-- Subtitle -->
        <p class="mx-auto mt-10 max-w-xl text-base leading-relaxed text-muted sm:text-lg md:text-xl">
          <!-- EDIT: Your subheadline -->
          Funding built around your goals, not the bank’s rules — with flexible options, faster approvals, and strategies designed to put you back in control of your finances.
        </p>

      </div>
    {/if}
  </div>


  
  <!-- Scroll indicator -->
  <!-- <div class="absolute bottom-8 left-1/2 -translate-x-1/2">
    <div class="flex h-10 w-6 items-start justify-center rounded-full border border-border p-2">
      <div class="h-1.5 w-1.5 animate-bounce rounded-full bg-muted" />
    </div>
  </div> -->
</section>