<script lang="ts">
  import { onMount } from 'svelte';
  import Equity from "$lib/assets/stratergy/5.webp"

  let sectionRef: HTMLElement;
  let flowRef: HTMLElement;
  let leftRef: HTMLElement;
  let rightRef: HTMLElement;

  const flowSteps = [
    {
      number: '01',
      title: 'Income Deposits Into HELOC',
      desc: 'Instead of sitting idle in a checking account, income immediately offsets principal balance.'
    },
    {
      number: '02',
      title: 'Interest Is Calculated Daily',
      desc: 'As the balance drops, interest charges shrink — creating accelerated payoff momentum.'
    },
    {
      number: '03',
      title: 'Expenses Flow Back Out',
      desc: 'You still use the funds normally, but the timing of cash flow changes the math dramatically.'
    },
    {
      number: '04',
      title: 'Principal Falls Faster',
      desc: 'The result is reduced interest drag and significantly faster mortgage elimination.'
    }
  ];

  onMount(() => {
    let ctx: any;

    const init = async () => {
      const gsap = (await import('gsap')).default;
      const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

      gsap.registerPlugin(ScrollTrigger);

      ctx = gsap.context(() => {
        gsap.from(leftRef, {
          opacity: 0,
          x: -80,
          duration: 1.2,
          ease: 'power4.out',
          scrollTrigger: {
            trigger: sectionRef,
            start: 'top 75%'
          }
        });

        gsap.from(rightRef, {
          opacity: 0,
          x: 80,
          duration: 1.2,
          ease: 'power4.out',
          scrollTrigger: {
            trigger: sectionRef,
            start: 'top 75%'
          }
        });

        gsap.from('.flow-step', {
          opacity: 0,
          y: 40,
          stagger: 0.15,
          duration: 0.9,
          ease: 'power3.out',
          scrollTrigger: {
            trigger: flowRef,
            start: 'top 80%'
          }
        });

        gsap.to('.flow-line', {
          backgroundPosition: '0% 200%',
          duration: 12,
          repeat: -1,
          ease: 'none'
        });

      }, sectionRef);
    };

    init();

    return () => {
      ctx?.revert();
    };
  });
</script>

<section
  bind:this={sectionRef}
  class="relative overflow-hidden bg-[#050505] py-32 lg:py-44"
>
  <!-- BACKGROUND -->
  <div class="absolute inset-0 overflow-hidden">
    <div class="absolute left-[-15%] top-[10%] h-[500px] w-[500px] rounded-full bg-[#c5a059]/[0.08] blur-[140px]"></div>

    <div class="absolute bottom-[-10%] right-[-10%] h-[600px] w-[600px] rounded-full bg-[#c5a059]/[0.06] blur-[160px]"></div>
  </div>

  <!-- GRID -->
  <div class="relative z-10 mx-auto grid max-w-7xl gap-20 px-6 lg:grid-cols-[0.9fr_1.1fr] lg:items-center">

    <!-- LEFT -->
    <div bind:this={leftRef}>
      <div class="inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl">
        <div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

        <span class="font-mono text-[11px] uppercase tracking-[0.32em] text-white/50">
          Equity Velocity System
        </span>
      </div>

      <h2
        class="mt-8 max-w-xl font-serif text-[clamp(3rem,5vw,5.5rem)] leading-[0.95] tracking-[-0.04em] text-white"
      >
        Your Income
        <span class="italic text-[#c5a059]">Becomes</span>
        The Weapon.
      </h2>

      <p class="mt-8 max-w-lg text-lg leading-relaxed text-white/60">
        Most homeowners use income inefficiently.
        Their money sits idle while interest compounds against them every day.

        This strategy changes the direction of cash flow.
      </p>

      <!-- FLOATING IMAGE -->
      <div class="relative mt-14 overflow-hidden rounded-[30px] border border-white/10">
        <img
          src={Equity}
          alt=""
          class="h-[420px] w-full object-cover"
        />

        <div class="absolute inset-0 bg-gradient-to-t from-black via-black/20 to-transparent"></div>

        <div class="absolute bottom-8 left-8 max-w-sm">
          <p class="font-mono text-[10px] uppercase tracking-[0.3em] text-white/40">
            Daily Interest Advantage
          </p>

          <p class="mt-3 text-2xl leading-snug text-white">
            Cash flow timing changes the speed of principal reduction.
          </p>
        </div>
      </div>
    </div>

    <!-- RIGHT -->
    <div
      bind:this={rightRef}
      bind:this={flowRef}
      class="relative"
    >
      <!-- FLOW LINE -->
      <div class="flow-line absolute left-[38px] top-0 h-full w-px bg-[linear-gradient(to_bottom,transparent,#c5a059,transparent)] bg-[length:100%_200%] opacity-40"></div>

      <div class="space-y-16">
        {#each flowSteps as step}
          <div class="flow-step relative pl-24">
            <!-- NUMBER -->
            <div class="absolute left-0 top-0 flex h-[78px] w-[78px] items-center justify-center rounded-full border border-white/10 bg-white/[0.03] backdrop-blur-xl">
              <span class="font-serif text-2xl text-[#c5a059]">
                {step.number}
              </span>
            </div>

            <div>
              <h3 class="text-3xl font-medium text-white">
                {step.title}
              </h3>

              <p class="mt-5 max-w-xl text-lg leading-relaxed text-white/55">
                {step.desc}
              </p>
            </div>
          </div>
        {/each}
      </div>
    </div>
  </div>
</section>