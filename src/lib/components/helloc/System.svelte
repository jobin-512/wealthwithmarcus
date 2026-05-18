<script lang="ts">
	import { onMount } from 'svelte';

	let sectionRef: HTMLElement;
	let visualRef: HTMLElement;
	let contentRef: HTMLElement;
	let statRefs: HTMLElement[] = [];

	const flow = [
		{
			step: '01',
			title: 'Income Deposits Into The HELOC',
			desc: 'Instead of parking your paycheck in a checking account earning almost nothing, your income flows directly into the HELOC balance.'
		},
		{
			step: '02',
			title: 'Average Daily Balance Drops',
			desc: 'Because HELOC interest is calculated daily, reducing the balance earlier lowers the amount of interest accruing every single day.'
		},
		{
			step: '03',
			title: 'Expenses Flow Back Out Strategically',
			desc: 'You still use your money normally — bills, spending, investments — but from a lower average balance position.'
		},
		{
			step: '04',
			title: 'Principal Falls Faster',
			desc: 'More of your money attacks principal immediately instead of waiting inside a traditional amortization schedule.'
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
					x: -80,
					opacity: 0,
					duration: 1.3,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 75%'
					}
				});

				gsap.from(visualRef, {
					x: 100,
					opacity: 0,
					scale: 0.9,
					duration: 1.5,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 75%'
					}
				});

				gsap.from(statRefs, {
					y: 60,
					opacity: 0,
					stagger: 0.14,
					duration: 1,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: statRefs[0],
						start: 'top 90%'
					}
				});

				// Floating graph line
				gsap.to('.pulse-line', {
					backgroundPosition: '200% 0%',
					duration: 10,
					repeat: -1,
					ease: 'none'
				});

				// Glow movement
				gsap.to('.orb-a', {
					x: 80,
					y: -50,
					duration: 10,
					repeat: -1,
					yoyo: true,
					ease: 'sine.inOut'
				});

				gsap.to('.orb-b', {
					x: -60,
					y: 70,
					duration: 12,
					repeat: -1,
					yoyo: true,
					ease: 'sine.inOut'
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
	class="relative overflow-hidden bg-[var(--theme-color)] py-32 lg:py-44"
>
	<!-- BACKGROUND -->
	<div class="noise"></div>

	<div class="orb-a"></div>
	<div class="orb-b"></div>

	<div class="vignette"></div>

	<!-- TOP LINE -->
	<div
		class="absolute inset-x-0 top-0 h-px bg-gradient-to-r from-transparent via-white/10 to-transparent"
	></div>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<div class="grid items-center gap-20 lg:grid-cols-[0.95fr_1.05fr]">
			<!-- LEFT -->
			<div bind:this={contentRef}>
				<div
					class="mb-7 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl"
				>
					<div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

					<span class="font-mono text-[11px] uppercase tracking-[0.34em] text-white/50">
						The System Behind First Lien HELOC
					</span>
				</div>

				<h2
					class="max-w-3xl font-serif text-[clamp(3rem,5vw,5.7rem)] leading-[0.95] tracking-[-0.04em] text-white"
				>
					Your Money
					<span class="italic text-[#c5a059]">
						Should Work Daily.
					</span>
				</h2>

				<p class="mt-8 max-w-2xl text-lg leading-relaxed text-white/58 lg:text-[1.15rem]">
					Traditional mortgages trap your income inside a slow amortization schedule.
					A First Lien HELOC changes the flow of money itself — reducing interest
					daily while accelerating principal reduction.
				</p>

				<div class="mt-14 space-y-7">
					{#each flow as item, i}
						<div
							bind:this={statRefs[i]}
							class="flow-row"
						>
							<div class="flow-number">
								{item.step}
							</div>

							<div class="min-w-0 flex-1">
								<h3 class="text-[1.05rem] font-medium text-white">
									{item.title}
								</h3>

								<p class="mt-2 max-w-xl text-sm leading-relaxed text-white/50">
									{item.desc}
								</p>
							</div>
						</div>
					{/each}
				</div>
			</div>

			<!-- RIGHT -->
			<div bind:this={visualRef} class="relative">
				<!-- MAIN VISUAL -->
				<div class="visual-wrap">
					<div class="glass-grid"></div>

					<!-- GRAPH -->
					<div class="graph-area">
						<div class="graph-top">
							<div>
								<p class="graph-label">
									Traditional Mortgage
								</p>

								<h3 class="graph-value red">
									30 Years
								</h3>
							</div>

							<div class="divider"></div>

							<div>
								<p class="graph-label">
									First Lien HELOC Strategy
								</p>

								<h3 class="graph-value gold">
									5–7 Years
								</h3>
							</div>
						</div>

						<!-- Animated Graph -->
						<div class="graph-box">
							<div class="graph-lines"></div>

							<div class="traditional-line"></div>

							<div class="heloc-line pulse-line"></div>

							<div class="graph-points">
								<div class="point a"></div>
								<div class="point b"></div>
							</div>
						</div>

						<div class="bottom-stats">
							<div>
								<p class="mini-label">Interest Reduction</p>
								<h4>Potentially Massive</h4>
							</div>

							<div>
								<p class="mini-label">Cash Flow Control</p>
								<h4>Daily Optimization</h4>
							</div>
						</div>
					</div>

					<!-- FLOATING PANEL -->
					<div class="floating-panel">
						<p class="font-mono text-[10px] uppercase tracking-[0.3em] text-white/40">
							Key Shift
						</p>

						<h4>
							You stop
							<span>renting money</span>
							from the bank.
						</h4>
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
		opacity: 0.03;
		background-image: radial-gradient(rgba(255,255,255,0.3) 0.5px, transparent 0.5px);
		background-size: 4px 4px;
		pointer-events: none;
	}

	.vignette {
		position: absolute;
		inset: 0;
		background:
			radial-gradient(circle at center,
			transparent 20%,
			rgba(0,0,0,0.82) 100%);
		pointer-events: none;
	}

	.orb-a,
	.orb-b {
		position: absolute;
		border-radius: 9999px;
		filter: blur(140px);
	}

	.orb-a {
		left: -200px;
		top: -100px;
		width: 600px;
		height: 600px;
		background: rgba(197,160,89,0.12);
	}

	.orb-b {
		right: -220px;
		bottom: -150px;
		width: 700px;
		height: 700px;
		background: rgba(197,160,89,0.08);
	}

	.flow-row {
		display: flex;
		align-items: flex-start;
		gap: 22px;
	}

	.flow-number {
		width: 58px;
		height: 58px;

		display: flex;
		align-items: center;
		justify-content: center;

		flex-shrink: 0;

		border-radius: 9999px;

		border: 1px solid rgba(255,255,255,0.08);

		background: rgba(255,255,255,0.03);

		font-family: monospace;
		font-size: 11px;
		letter-spacing: 0.22em;
		color: #c5a059;
	}

	.visual-wrap {
		position: relative;
		overflow: hidden;

		border-radius: 40px;

		border: 1px solid rgba(255,255,255,0.08);

		background:
			linear-gradient(
				180deg,
				rgba(255,255,255,0.05),
				rgba(255,255,255,0.02)
			);

		padding: 40px;

		backdrop-filter: blur(28px);

		min-height: 720px;
	}

	.glass-grid {
		position: absolute;
		inset: 0;

		background-image:
			linear-gradient(rgba(255,255,255,0.04) 1px, transparent 1px),
			linear-gradient(90deg, rgba(255,255,255,0.04) 1px, transparent 1px);

		background-size: 50px 50px;

		mask-image: radial-gradient(circle at center, black 40%, transparent 90%);
	}

	.graph-area {
		position: relative;
		z-index: 2;
	}

	.graph-top {
		display: flex;
		align-items: center;
		justify-content: space-between;
		gap: 30px;
	}

	.divider {
		flex: 1;
		height: 1px;
		background: rgba(255,255,255,0.08);
	}

	.graph-label {
		font-size: 10px;
		letter-spacing: 0.28em;
		text-transform: uppercase;
		color: rgba(255,255,255,0.4);
		font-family: monospace;
		margin-bottom: 10px;
	}

	.graph-value {
		font-family: serif;
		font-size: clamp(2rem, 4vw, 3.5rem);
		line-height: 1;
	}

	.graph-value.red {
		color: rgba(255,255,255,0.35);
	}

	.graph-value.gold {
		color: #c5a059;
	}

	.graph-box {
		position: relative;
		height: 340px;
		margin-top: 60px;
		border-radius: 30px;
		border: 1px solid rgba(255,255,255,0.08);
		background: rgba(255,255,255,0.03);
		overflow: hidden;
	}

	.graph-lines {
		position: absolute;
		inset: 0;

		background-image:
			linear-gradient(rgba(255,255,255,0.04) 1px, transparent 1px),
			linear-gradient(90deg, rgba(255,255,255,0.04) 1px, transparent 1px);

		background-size: 60px 60px;
	}

	.traditional-line,
	.heloc-line {
		position: absolute;
		left: 60px;
		right: 60px;
		height: 4px;
		border-radius: 999px;
	}

	.traditional-line {
		top: 120px;
		background: rgba(255,255,255,0.22);
		transform: rotate(8deg);
		transform-origin: left;
	}

	.heloc-line {
		bottom: 90px;

		background:
			linear-gradient(
				90deg,
				rgba(197,160,89,0.4),
				#c5a059,
				rgba(197,160,89,0.4)
			);

		background-size: 200% 100%;

		box-shadow: 0 0 30px rgba(197,160,89,0.35);

		transform: rotate(-10deg);
		transform-origin: left;
	}

	.graph-points .point {
		position: absolute;
		width: 16px;
		height: 16px;
		border-radius: 9999px;
	}

	.point.a {
		top: 100px;
		right: 100px;
		background: rgba(255,255,255,0.35);
	}

	.point.b {
		bottom: 70px;
		right: 120px;
		background: #c5a059;
		box-shadow: 0 0 25px rgba(197,160,89,0.5);
	}

	.bottom-stats {
		display: grid;
		grid-template-columns: repeat(2, minmax(0, 1fr));
		gap: 24px;
		margin-top: 32px;
	}

	.mini-label {
		font-size: 10px;
		letter-spacing: 0.24em;
		text-transform: uppercase;
		color: rgba(255,255,255,0.35);
		font-family: monospace;
		margin-bottom: 10px;
	}

	.bottom-stats h4 {
		font-size: 1.15rem;
		color: white;
	}

	.floating-panel {
		position: absolute;
		right: 30px;
		bottom: 30px;

		max-width: 260px;

		padding: 24px;

		border-radius: 26px;

		border: 1px solid rgba(255,255,255,0.08);

		background: rgba(255,255,255,0.04);

		backdrop-filter: blur(20px);

		box-shadow:
			0 20px 60px rgba(0,0,0,0.45);
	}

	.floating-panel h4 {
		margin-top: 14px;

		font-size: 1.25rem;
		line-height: 1.5;

		color: rgba(255,255,255,0.85);
	}

	.floating-panel span {
		color: #c5a059;
		font-style: italic;
	}

	@media (max-width: 768px) {
		.visual-wrap {
			min-height: auto;
			padding: 26px;
		}

		.graph-top {
			flex-direction: column;
			align-items: flex-start;
		}

		.divider {
			width: 100%;
		}

		.graph-box {
			height: 240px;
		}

		.bottom-stats {
			grid-template-columns: 1fr;
		}

		.floating-panel {
			position: relative;
			right: auto;
			bottom: auto;
			margin-top: 28px;
			max-width: 100%;
		}
	}
</style>