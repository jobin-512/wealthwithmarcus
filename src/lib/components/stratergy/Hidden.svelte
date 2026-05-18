<script lang="ts">
	import { onMount } from 'svelte';
    import Finance from "$lib/assets/stratergy/2.webp"

	let sectionRef: HTMLElement;
	let imageWrap: HTMLElement;
	let stickyRef: HTMLElement;
	let cards: HTMLElement[] = [];

	const breakdown = [
		{
			step: '01',
			title: 'Traditional Mortgage Thinking',
			desc:
				'Most homeowners are trained to think only about interest rate. So they refinance repeatedly, restart 30-year amortization schedules, and stay trapped in long-term debt.',
			stat: '30 YEARS'
		},
		{
			step: '02',
			title: 'Cash Flow Is The Real Lever',
			desc:
				'The real issue is idle money. Every dollar sitting in checking accounts loses efficiency while mortgage interest compounds daily against you.',
			stat: 'DAILY INTEREST'
		},
		{
			step: '03',
			title: 'Your Equity Should Work',
			desc:
				'Instead of locking equity away, strategic lending structures allow your income to actively attack principal while preserving liquidity and control.',
			stat: 'ACTIVE CAPITAL'
		},
		{
			step: '04',
			title: 'Velocity Changes Everything',
			desc:
				'When cash flow moves through the right structure, principal declines faster, interest exposure drops, and timelines compress dramatically.',
			stat: '5–7 YEARS'
		}
	];

	onMount(() => {
		let cleanup: (() => void) | undefined;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			const ctx = gsap.context(() => {
				gsap.from('.strategy-label', {
					y: 24,
					opacity: 0,
					duration: 0.9,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 82%'
					}
				});

				gsap.from('.strategy-title .line', {
					yPercent: 120,
					opacity: 0,
					stagger: 0.08,
					duration: 1.2,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 78%'
					}
				});

				gsap.from(imageWrap, {
					scale: 0.88,
					opacity: 0,
					duration: 1.5,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: imageWrap,
						start: 'top 82%'
					}
				});

				gsap.from(cards, {
					y: 80,
					opacity: 0,
					stagger: 0.12,
					duration: 1,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: stickyRef,
						start: 'top 75%'
					}
				});

				gsap.to(imageWrap, {
					yPercent: -12,
					ease: 'none',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top bottom',
						end: 'bottom top',
						scrub: true
					}
				});
			}, sectionRef);

			cleanup = () => ctx.revert();
		};

		init();

		return () => cleanup?.();
	});
</script>

<section
	bind:this={sectionRef}
	class="relative overflow-hidden bg-[var(--theme-color)] py-32 lg:py-44"
>
	<!-- Background -->
	<div class="noise"></div>

	<div class="absolute inset-0">
		<div
			class="absolute left-[-10%] top-[10%] h-[520px] w-[520px] rounded-full bg-[#c5a059]/[0.08] blur-[120px]"
		></div>

		<div
			class="absolute bottom-[-10%] right-[-5%] h-[600px] w-[600px] rounded-full bg-[#c5a059]/[0.05] blur-[140px]"
		></div>
	</div>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<!-- Heading -->
		<div class="mb-20 max-w-4xl">
			<div
				class="strategy-label mb-8 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl"
			>
				<div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

				<span class="font-mono text-[11px] uppercase tracking-[0.32em] text-white/55">
					The Financial System Nobody Explains
				</span>
			</div>

			<h2
				class="strategy-title overflow-hidden font-serif text-[clamp(3rem,6vw,6.5rem)] leading-[0.95] tracking-[-0.05em] text-white"
			>
				<div class="line">The Problem</div>

				<div class="line text-white/40">Isn’t Your Rate.</div>

				<div class="line italic text-[#c5a059]">It’s The Structure.</div>
			</h2>

			<p class="mt-10 max-w-2xl text-lg leading-relaxed text-white/55 lg:text-xl">
				Most people spend decades optimizing interest rates while completely ignoring
				the movement of cash flow. That’s why even “good loans” still create long-term
				financial drag.
			</p>
		</div>

		<!-- Visual Split -->
		<div class="grid grid-cols-1 gap-20 lg:grid-cols-[0.9fr_1.1fr]">
			<!-- Image -->
			<div class="relative">
				<div
					bind:this={stickyRef}
					class="sticky top-28"
				>
					<div
						bind:this={imageWrap}
						class="relative overflow-hidden rounded-[34px] border border-white/10"
					>
						<img
							src={Finance}
							alt="Financial strategy"
							class="h-[680px] w-full object-cover"
						/>

						<!-- Overlays -->
						<div
							class="absolute inset-0 bg-gradient-to-t from-black via-black/20 to-transparent"
						></div>

						<div
							class="absolute inset-0 bg-[radial-gradient(circle_at_top,rgba(197,160,89,0.18),transparent_55%)]"
						></div>

						<!-- Floating quote -->
						<div
							class="absolute bottom-8 left-8 right-8 rounded-[28px] border border-white/10 bg-black/40 p-7 backdrop-blur-2xl"
						>
							<p
								class="mb-5 font-mono text-[10px] uppercase tracking-[0.28em] text-[#c5a059]"
							>
								The Real Shift
							</p>

							<p class="text-2xl leading-[1.35] text-white">
								“Financial freedom doesn’t come from chasing lower rates.
								It comes from controlling how money moves.”
							</p>
						</div>
					</div>
				</div>
			</div>

			<!-- Breakdown -->
			<div class="space-y-10">
				{#each breakdown as item, i}
					<div
						bind:this={cards[i]}
						class="group relative border-b border-white/10 pb-10"
					>
						<div class="mb-6 flex items-center justify-between gap-8">
							<div
								class="font-mono text-[11px] uppercase tracking-[0.35em] text-[#c5a059]"
							>
								{item.step}
							</div>

							<div
								class="rounded-full border border-white/10 bg-white/[0.03] px-5 py-2 font-mono text-[10px] uppercase tracking-[0.25em] text-white/40 transition-all duration-500 group-hover:border-[#c5a059]/30 group-hover:text-[#c5a059]"
							>
								{item.stat}
							</div>
						</div>

						<h3
							class="mb-5 text-[2rem] leading-none tracking-[-0.03em] text-white transition-all duration-500 group-hover:translate-x-2"
						>
							{item.title}
						</h3>

						<p
							class="max-w-xl text-[15px] leading-[1.9] text-white/50 transition-all duration-500 group-hover:text-white/70"
						>
							{item.desc}
						</p>

						<div
							class="absolute bottom-0 left-0 h-px w-0 bg-[#c5a059] transition-all duration-700 group-hover:w-full"
						></div>
					</div>
				{/each}
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
		background-image: radial-gradient(rgba(255,255,255,0.3) 0.5px, transparent 0.5px);
		background-size: 4px 4px;
	}
</style>