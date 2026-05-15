<script lang="ts">
	import { onMount } from 'svelte';

    import Strategy from "$lib/assets/results/3.webp"

	let sectionRef: HTMLElement;
	let leftRef: HTMLElement;
	let rightRef: HTMLElement;
	let timelineRefs: HTMLElement[] = [];

	const scenarios = [
		{
			title: 'The “Locked-In” Homeowner',
			before: {
				label: 'Before',
				points: [
					'2.9% mortgage rate',
					'Needed liquidity but refused to refinance',
					'High-interest consumer debt',
					'Cash trapped inside equity'
				]
			},
			after: {
				label: 'After',
				points: [
					'Added a Digital HELOC without touching first mortgage',
					'Consolidated revolving debt',
					'Reduced monthly interest burden',
					'Created flexible liquidity access'
				]
			},
			result: '$2,940/month improved cash flow'
		},
		{
			title: 'The High-Net-Worth Retiree',
			before: {
				label: 'Before',
				points: [
					'$3.2M home equity',
					'Large retirement withdrawals',
					'Concerned about market timing',
					'Wanted tax-efficient liquidity'
				]
			},
			after: {
				label: 'After',
				points: [
					'Structured proprietary reverse strategy',
					'Eliminated mandatory mortgage payments',
					'Protected investment portfolio',
					'Accessed liquidity without selling assets'
				]
			},
			result: '$740K strategic liquidity created'
		}
	];

	onMount(() => {
		let cleanup: (() => void) | undefined;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			const ctx = gsap.context(() => {
				gsap.from(leftRef, {
					x: -100,
					opacity: 0,
					duration: 1.2,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 75%'
					}
				});

				gsap.from(rightRef, {
					x: 100,
					opacity: 0,
					duration: 1.2,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 75%'
					}
				});

				gsap.from(timelineRefs, {
					y: 70,
					opacity: 0,
					stagger: 0.16,
					duration: 1,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 72%'
					}
				});
			}, sectionRef);

			cleanup = () => {
				ctx.revert();
			};
		};

		init();

		return () => {
			cleanup?.();
		};
	});
</script>

<section
	bind:this={sectionRef}
	class="relative overflow-hidden bg-[#070707] py-32 lg:py-44"
>
	<!-- BG -->
	<div class="noise"></div>

	<div class="mesh mesh-1"></div>
	<div class="mesh mesh-2"></div>

	<div class="vignette"></div>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<div class="grid grid-cols-1 gap-24 lg:grid-cols-[0.9fr_1.1fr]">
			<!-- LEFT -->
			<div bind:this={leftRef}>
				<div
					class="mb-7 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl"
				>
					<div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

					<span class="font-mono text-[11px] uppercase tracking-[0.35em] text-white/45">
						Real Strategy Outcomes
					</span>
				</div>

				<h2
					class="max-w-xl font-serif text-[clamp(3rem,5vw,5.7rem)] leading-[0.94] tracking-[-0.05em] text-white"
				>
					These Aren’t
					<span class="italic text-[#c5a059]"> Mortgage Cases.</span>

					<br />

					They’re
					<span class="italic text-[#c5a059]"> Financial Restructures.</span>
				</h2>

				<p class="mt-10 max-w-lg text-lg leading-relaxed text-white/55">
					The difference between a loan officer and a strategist is simple:
					one sells products — the other redesigns outcomes.
				</p>

				<div class="mt-16">
					<img
						src={Strategy}
						alt=""
						class="h-[540px] w-full rounded-[34px] object-cover"
					/>
				</div>
			</div>

			<!-- RIGHT -->
			<div bind:this={rightRef} class="space-y-12">
				{#each scenarios as item, i}
					<div
						bind:this={timelineRefs[i]}
						class="scenario"
					>
						<div class="scenario-line"></div>

						<div class="relative z-10">
							<div class="mb-8 flex items-center justify-between gap-6">
								<h3
									class="max-w-md font-serif text-3xl leading-tight tracking-[-0.03em] text-white"
								>
									{item.title}
								</h3>

								<div
									class="rounded-full border border-[#c5a059]/20 bg-[#c5a059]/10 px-5 py-3 text-sm font-medium text-[#e8c98d]"
								>
									{item.result}
								</div>
							</div>

							<div class="grid gap-10 md:grid-cols-2">
								<!-- BEFORE -->
								<div>
									<p
										class="mb-5 font-mono text-[11px] uppercase tracking-[0.3em] text-white/35"
									>
										{item.before.label}
									</p>

									<div class="space-y-4">
										{#each item.before.points as point}
											<div class="flex items-start gap-4">
												<div
													class="mt-2 h-1.5 w-1.5 rounded-full bg-white/25"
												></div>

												<p class="text-sm leading-relaxed text-white/50">
													{point}
												</p>
											</div>
										{/each}
									</div>
								</div>

								<!-- AFTER -->
								<div>
									<p
										class="mb-5 font-mono text-[11px] uppercase tracking-[0.3em] text-[#c5a059]"
									>
										{item.after.label}
									</p>

									<div class="space-y-4">
										{#each item.after.points as point}
											<div class="flex items-start gap-4">
												<div
													class="mt-2 h-1.5 w-1.5 rounded-full bg-[#c5a059]"
												></div>

												<p class="text-sm leading-relaxed text-white/72">
													{point}
												</p>
											</div>
										{/each}
									</div>
								</div>
							</div>
						</div>
					</div>
				{/each}
			</div>
		</div>
	</div>
</section>

<style>
	:global(body) {
		background: #070707;
	}

	.noise {
		position: absolute;
		inset: 0;
		opacity: 0.02;
		pointer-events: none;
		background-image:
			radial-gradient(rgba(255,255,255,0.3) 0.5px, transparent 0.5px);

		background-size: 4px 4px;
	}

	.vignette {
		position: absolute;
		inset: 0;

		background:
			radial-gradient(circle at center,
			transparent 25%,
			rgba(0,0,0,0.85) 100%);
	}

	.mesh {
		position: absolute;
		border-radius: 9999px;
		filter: blur(140px);
	}

	.mesh-1 {
		left: -200px;
		top: -200px;

		width: 700px;
		height: 700px;

		background: rgba(197,160,89,0.1);
	}

	.mesh-2 {
		right: -300px;
		bottom: -300px;

		width: 900px;
		height: 900px;

		background: rgba(197,160,89,0.07);
	}

	.scenario {
		position: relative;

		padding-left: 44px;
		padding-bottom: 50px;
	}

	.scenario:last-child {
		padding-bottom: 0;
	}

	.scenario-line {
		position: absolute;
		left: 0;
		top: 8px;
		bottom: 0;

		width: 1px;

		background:
			linear-gradient(
				to bottom,
				rgba(197,160,89,0.5),
				rgba(255,255,255,0.06)
			);
	}

	.scenario::before {
		content: '';

		position: absolute;
		left: -6px;
		top: 0;

		width: 14px;
		height: 14px;

		border-radius: 9999px;

		background: #c5a059;

		box-shadow:
			0 0 20px rgba(197,160,89,0.5);
	}
</style>