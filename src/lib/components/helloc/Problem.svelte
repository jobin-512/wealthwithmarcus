<script lang="ts">
	import { onMount } from 'svelte';
    import Problem from '$lib/assets/helloc/3.webp';

	let sectionRef: HTMLElement;
	let imageRef: HTMLElement;
	let contentRef: HTMLElement;
	let graphRef: HTMLElement;

	const comparisons = [
		{
			label: 'Traditional Mortgage',
			old: 'Interest-heavy structure',
			new: 'Minimal principal reduction early on'
		},
		{
			label: 'Refinancing',
			old: 'Restarts the amortization clock',
			new: 'Extends debt longer'
		},
		{
			label: 'First Lien HELOC',
			old: 'Cash flow attacks principal daily',
			new: 'Accelerated payoff structure'
		}
	];

	onMount(() => {
		let ctx: any;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			ctx = gsap.context(() => {
				gsap.from(contentRef, {
					y: 80,
					opacity: 0,
					duration: 1.2,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 72%'
					}
				});

				gsap.from(imageRef, {
					scale: 0.9,
					opacity: 0,
					duration: 1.4,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 72%'
					}
				});

				gsap.from('.compare-row', {
					x: 40,
					opacity: 0,
					stagger: 0.12,
					duration: 0.8,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: graphRef,
						start: 'top 80%'
					}
				});

				gsap.to('.floating-orb', {
					y: -20,
					duration: 4,
					repeat: -1,
					yoyo: true,
					ease: 'sine.inOut'
				});
			}, sectionRef);
		};

		init();

		return () => {
			ctx?.revert?.();
		};
	});
</script>

<section
	bind:this={sectionRef}
	class="relative overflow-hidden bg-[#040404] py-32 lg:py-44"
>
	<!-- Background -->
	<div class="absolute inset-0 overflow-hidden">
		<div class="floating-orb absolute left-[8%] top-[20%] h-[280px] w-[280px] rounded-full bg-[#c5a059]/[0.08] blur-[120px]"></div>

		<div class="absolute bottom-[-10%] right-[5%] h-[420px] w-[420px] rounded-full bg-[#c5a059]/[0.06] blur-[140px]"></div>
	</div>

	<div class="noise"></div>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		
		<!-- TOP -->
		<div
			bind:this={contentRef}
			class="mx-auto max-w-4xl text-center"
		>
			<div class="mb-8 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl">
				<div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

				<span class="font-mono text-[11px] uppercase tracking-[0.34em] text-white/50">
					The Real Problem
				</span>
			</div>

			<h2
				class="font-serif text-[clamp(3rem,6vw,6rem)] leading-[0.95] tracking-[-0.04em] text-white"
			>
				Most Homeowners
				<span class="italic text-[#c5a059]">Never Realize</span>
				How The Bank Wins
			</h2>

			<p class="mx-auto mt-10 max-w-3xl text-xl leading-relaxed text-white/58">
				The traditional mortgage system is designed so the majority of your
				early payments go toward interest — not principal.
			</p>

			<p class="mx-auto mt-6 max-w-2xl text-lg leading-relaxed text-white/45">
				That means years of payments with very little actual debt reduction.
			</p>
		</div>

		<!-- VISUAL SECTION -->
		<div class="mt-24 grid grid-cols-1 gap-14 lg:grid-cols-[1fr_0.95fr] lg:items-center">
			
			<!-- IMAGE -->
			<div
				bind:this={imageRef}
				class="relative overflow-hidden rounded-[38px] border border-white/10"
			>
				<img
					src={Problem}
					alt="Financial strategy"
					class="h-[720px] w-full object-cover"
				/>

				<!-- Overlay -->
				<div class="absolute inset-0 bg-gradient-to-t from-black via-black/10 to-transparent"></div>

				<!-- Floating Data -->
				<div class="absolute bottom-8 left-8 right-8">
					<div class="rounded-[28px] border border-white/10 bg-black/40 p-8 backdrop-blur-2xl">
						<p class="font-mono text-[11px] uppercase tracking-[0.28em] text-white/40">
							Traditional Mortgage Structure
						</p>

						<div class="mt-6 flex items-end gap-4">
							<h3 class="font-serif text-7xl leading-none text-white">
								80%
							</h3>

							<p class="max-w-[180px] pb-2 text-sm leading-relaxed text-white/50">
								of early payments often go toward interest
							</p>
						</div>

						<div class="mt-8 h-px bg-white/10"></div>

						<p class="mt-6 text-base leading-relaxed text-white/55">
							You are making payments —
							but your balance barely moves.
						</p>
					</div>
				</div>
			</div>

			<!-- RIGHT -->
			<div bind:this={graphRef}>
				<div class="space-y-8">
					{#each comparisons as item}
						<div
							class="compare-row rounded-[30px] border border-white/10 bg-white/[0.03] p-8 backdrop-blur-xl"
						>
							<p class="font-mono text-[11px] uppercase tracking-[0.26em] text-[#c5a059]">
								{item.label}
							</p>

							<div class="mt-6 grid grid-cols-1 gap-5 sm:grid-cols-2">
								
								<div class="rounded-2xl border border-red-500/10 bg-red-500/[0.04] p-5">
									<p class="mb-3 text-xs uppercase tracking-[0.2em] text-red-300/60">
										Old Structure
									</p>

									<p class="text-base leading-relaxed text-white/75">
										{item.old}
									</p>
								</div>

								<div class="rounded-2xl border border-[#c5a059]/15 bg-[#c5a059]/[0.05] p-5">
									<p class="mb-3 text-xs uppercase tracking-[0.2em] text-[#c5a059]/70">
										New Approach
									</p>

									<p class="text-base leading-relaxed text-white/85">
										{item.new}
									</p>
								</div>
							</div>
						</div>
					{/each}
				</div>

				<!-- Quote -->
				<div class="mt-10 border-l border-[#c5a059]/40 pl-6">
					<p class="font-serif text-3xl leading-[1.3] tracking-tight text-white">
						“The issue isn’t your rate.
						It’s the structure behind how debt compounds over time.”
					</p>
				</div>
			</div>
		</div>
	</div>
</section>

<style>
	.noise {
		position: absolute;
		inset: 0;
		opacity: 0.025;
		background-image:
			radial-gradient(rgba(255,255,255,0.35) 0.5px, transparent 0.5px);
		background-size: 4px 4px;
		pointer-events: none;
	}
</style>