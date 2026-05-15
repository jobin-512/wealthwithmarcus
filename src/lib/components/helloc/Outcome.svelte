<script lang="ts">
	import { onMount } from 'svelte';
    import Outcome from "$lib/assets/helloc/7.webp";

	let sectionRef: HTMLElement;
	let imageWrap: HTMLElement;
	let contentWrap: HTMLElement;
	let statsWrap: HTMLElement;

	const outcomes = [
		{
			number: '67%',
			label: 'Average monthly cash-flow improvement'
		},
		{
			number: '$412K',
			label: 'Average accessible equity unlocked'
		},
		{
			number: '5-7Y',
			label: 'Projected payoff acceleration strategy'
		}
	];

	onMount(() => {
		let ctx: any;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			ctx = gsap.context(() => {
				gsap.from(contentWrap, {
					y: 90,
					opacity: 0,
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
					rotate: -3,
					duration: 1.4,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: imageWrap,
						start: 'top 82%'
					}
				});

				gsap.from('.outcome-item', {
					y: 40,
					opacity: 0,
					stagger: 0.12,
					duration: 1,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: statsWrap,
						start: 'top 90%'
					}
				});

				gsap.to('.parallax-img', {
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
	<!-- Background -->
	<div class="noise"></div>

	<div class="absolute inset-0">
		<div
			class="absolute left-[-10%] top-[10%] h-[500px] w-[500px] rounded-full bg-[#c5a059]/[0.08] blur-[130px]"
		></div>

		<div
			class="absolute bottom-[-10%] right-[-10%] h-[520px] w-[520px] rounded-full bg-white/[0.03] blur-[150px]"
		></div>
	</div>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<div class="grid items-center gap-20 lg:grid-cols-[1fr_0.9fr]">
			<!-- LEFT -->
			<div bind:this={contentWrap}>
				<div
					class="mb-8 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl"
				>
					<div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

					<span
						class="font-mono text-[11px] uppercase tracking-[0.32em] text-white/55"
					>
						Financial Outcome Engineering
					</span>
				</div>

				<h2
					class="max-w-3xl font-serif text-[clamp(3rem,5vw,5.8rem)] leading-[0.95] tracking-[-0.04em] text-white"
				>
					This Isn’t About
					<span class="italic text-[#c5a059]">Getting Another Loan.</span>
				</h2>

				<p
					class="mt-10 max-w-2xl text-lg leading-relaxed text-white/60 lg:text-xl"
				>
					It’s about restructuring how money moves through your life.
					The right lending strategy doesn’t just lower pressure —
					it increases flexibility, preserves liquidity, and gives you control
					over your future decisions.
				</p>

				<p
					class="mt-6 max-w-2xl text-base leading-relaxed text-white/42"
				>
					Most banks sell products.
					We build systems designed around cash flow behavior, equity positioning,
					and long-term financial leverage.
				</p>

				<div
					bind:this={statsWrap}
					class="mt-16 grid gap-8 sm:grid-cols-3"
				>
					{#each outcomes as item}
						<div class="outcome-item">
							<div
								class="mb-2 font-serif text-4xl tracking-[-0.04em] text-white"
							>
								{item.number}
							</div>

							<p
								class="max-w-[180px] text-sm leading-relaxed text-white/45"
							>
								{item.label}
							</p>
						</div>
					{/each}
				</div>
			</div>

			<!-- RIGHT -->
			<div bind:this={imageWrap} class="relative">
				<div
					class="absolute inset-0 scale-[1.05] rounded-[34px] bg-[#c5a059]/10 blur-[80px]"
				></div>

				<div
					class="relative overflow-hidden rounded-[34px] border border-white/10 bg-white/[0.03]"
				>
					<div class="absolute inset-0 bg-gradient-to-t from-black/70 to-transparent"></div>

					<img
						src={Outcome}
						alt="Luxury financial strategy"
						class="parallax-img h-[720px] w-full object-cover"
					/>

					<div class="absolute bottom-0 left-0 right-0 p-8 lg:p-10">
						<div
							class="mb-4 inline-flex items-center gap-2 rounded-full border border-white/10 bg-black/30 px-4 py-2 backdrop-blur-xl"
						>
							<div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

							<span
								class="font-mono text-[10px] uppercase tracking-[0.3em] text-white/60"
							>
								Equity Strategy
							</span>
						</div>

						<h3
							class="max-w-md font-serif text-3xl leading-tight text-white"
						>
							Use your income to build momentum —
							not just make payments.
						</h3>
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
		opacity: 0.025;
		pointer-events: none;
		background-image:
			radial-gradient(rgba(255,255,255,0.3) 0.5px, transparent 0.5px);
		background-size: 4px 4px;
	}

	img {
		will-change: transform;
	}

	@media (max-width: 768px) {
		img {
			height: 520px !important;
		}
	}
</style>