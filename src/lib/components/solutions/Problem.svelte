<script lang="ts">
	import { onMount } from 'svelte';

	let sectionRef: HTMLElement;
	let headlineRef: HTMLElement;
	let lineRef: HTMLElement;
	let statRefs: HTMLElement[] = $state([]);
	let textRefs: HTMLElement[] = $state([]);

	const payoffData = [
		{
			label: 'Traditional Mortgage',
			years: '30 Years',
			desc: 'Most homeowners stay trapped in long-term amortization cycles.'
		},
		{
			label: 'Refinance Reset',
			years: '25–30 Years',
			desc: 'Lower payment. Same system. Same interest structure.'
		},
		{
			label: 'First Lien HELOC Strategy',
			years: '5–7 Years',
			desc: 'Cash flow optimized. Interest minimized. Principal attacked daily.'
		}
	];

	onMount(() => {
		let cleanup: (() => void) | undefined;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			const ctx = gsap.context(() => {
				gsap.from(headlineRef.children, {
					y: 90,
					opacity: 0,
					rotateX: -70,
					stagger: 0.06,
					duration: 1.2,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: headlineRef,
						start: 'top 82%'
					}
				});

				gsap.from(lineRef, {
					scaleX: 0,
					transformOrigin: 'left center',
					duration: 1.4,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: lineRef,
						start: 'top 90%'
					}
				});

				gsap.from(statRefs, {
					y: 80,
					opacity: 0,
					stagger: 0.18,
					duration: 1.1,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: statRefs[0],
						start: 'top 85%'
					}
				});

				gsap.from(textRefs, {
					y: 40,
					opacity: 0,
					stagger: 0.12,
					duration: 1,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: textRefs[0],
						start: 'top 90%'
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
	class="relative overflow-hidden bg-[#050505] py-32 lg:py-44"
>
	<!-- background -->
	<div class="noise"></div>

	<div class="absolute inset-0">
		<div
			class="absolute left-[-15%] top-[10%] h-[700px] w-[700px] rounded-full bg-[#c5a059]/[0.08] blur-[140px]"
		></div>

		<div
			class="absolute bottom-[-10%] right-[-10%] h-[800px] w-[800px] rounded-full bg-[#c5a059]/[0.05] blur-[160px]"
		></div>
	</div>

	<div class="vignette"></div>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<!-- top -->
		<div class="grid gap-16 lg:grid-cols-[1fr_0.8fr] lg:gap-24">
			<div>
				<div
					class="mb-7 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl"
				>
					<div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

					<span class="font-mono text-[11px] uppercase tracking-[0.32em] text-white/50">
						The Real Problem Isn’t The Rate
					</span>
				</div>

				<h2
					bind:this={headlineRef}
					class="max-w-5xl font-serif text-[clamp(3rem,6vw,6.5rem)] leading-[0.92] tracking-[-0.05em] text-white"
					style="perspective:1000px;"
				>
					<span class="mr-[0.18em] inline-block">Most</span>
					<span class="mr-[0.18em] inline-block text-[#c5a059]">Homeowners</span>
					<span class="mr-[0.18em] inline-block">Are</span>
					<span class="mr-[0.18em] inline-block">Paying</span>

					<br />

					<span class="mr-[0.18em] inline-block">Interest</span>
					<span class="mr-[0.18em] inline-block">Like</span>
					<span class="mr-[0.18em] inline-block italic text-[#c5a059]">
						Rent.
					</span>
				</h2>
			</div>

			<div class="flex flex-col justify-end">
				<p
					bind:this={textRefs[0]}
					class="max-w-xl text-lg leading-relaxed text-white/58 lg:text-xl"
				>
					The banking system rewards slow repayment.
					Most people focus on the rate —
					while ignoring how interest compounds over time.
				</p>

				<div
					bind:this={lineRef}
					class="my-10 h-px w-full bg-gradient-to-r from-[#c5a059] via-white/20 to-transparent"
				></div>

				<p
					bind:this={textRefs[1]}
					class="max-w-xl text-base leading-relaxed text-white/42"
				>
					The right structure changes everything:
					cash flow, payoff speed, liquidity, and long-term wealth retention.
				</p>
			</div>
		</div>

		<!-- comparison -->
		<div class="mt-28">
			<div class="comparison-wrap">
				{#each payoffData as item, i}
					<div
						bind:this={statRefs[i]}
						class="comparison-row"
					>
						<div class="left">
							<p class="mini">
								0{i + 1}
							</p>

							<h3>
								{item.label}
							</h3>
						</div>

						<div class="middle">
							<div class="timeline-line">
								<div
									class="timeline-fill"
									style={`width:${i === 0 ? '100%' : i === 1 ? '85%' : '24%'}`}
								></div>
							</div>
						</div>

						<div class="right">
							<div class="year-wrap">
								<span class:gold={i === 2}>
									{item.years}
								</span>
							</div>

							<p>
								{item.desc}
							</p>
						</div>
					</div>
				{/each}
			</div>
		</div>

		<!-- bottom statement -->
		<div class="mt-28 grid gap-12 border-t border-white/10 pt-16 lg:grid-cols-2">
			<div>
				<p
					bind:this={textRefs[2]}
					class="font-serif text-[clamp(2rem,4vw,3.6rem)] leading-[1.02] tracking-[-0.04em] text-white"
				>
					You don’t need
					<span class="italic text-[#c5a059]">another loan.</span>

					<br />

					You need a better
					<span class="italic text-[#c5a059]">system.</span>
				</p>
			</div>

			<div class="flex items-end">
				<p
					bind:this={textRefs[3]}
					class="max-w-2xl text-lg leading-relaxed text-white/52"
				>
					Our strategies are designed around liquidity, velocity, and equity access —
					not trapping you in another 30-year cycle.
				</p>
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
		background-image: radial-gradient(rgba(255,255,255,0.4) 0.5px, transparent 0.5px);
		background-size: 4px 4px;
		pointer-events: none;
	}

	.vignette {
		position: absolute;
		inset: 0;
		pointer-events: none;

		background:
			radial-gradient(circle at center,
			transparent 20%,
			rgba(0,0,0,0.82) 100%);
	}

	.comparison-wrap {
		border-top: 1px solid rgba(255,255,255,0.08);
	}

	.comparison-row {
		display: grid;
		grid-template-columns: 1.2fr 1fr 1fr;
		gap: 40px;

		padding: 42px 0;

		border-bottom: 1px solid rgba(255,255,255,0.08);
	}

	.left {
		display: flex;
		align-items: center;
		gap: 24px;
		min-width: 0;
	}

	.mini {
		font-size: 11px;
		letter-spacing: 0.28em;
		text-transform: uppercase;
		color: rgba(255,255,255,0.28);
		font-family: monospace;
		flex-shrink: 0;
	}

	.left h3 {
		font-size: clamp(1.4rem, 2vw, 2rem);
		line-height: 1.1;
		letter-spacing: -0.03em;
		color: white;
		font-family: serif;
		word-break: break-word;
	}

	.middle {
		display: flex;
		align-items: center;
	}

	.timeline-line {
		position: relative;
		width: 100%;
		height: 2px;
		background: rgba(255,255,255,0.08);
		overflow: hidden;
		border-radius: 999px;
	}

	.timeline-fill {
		position: absolute;
		left: 0;
		top: 0;
		height: 100%;

		background:
			linear-gradient(
				90deg,
				#c5a059,
				rgba(255,255,255,0.85)
			);

		border-radius: 999px;
	}

	.right {
		min-width: 0;
	}

	.year-wrap {
		margin-bottom: 12px;
	}

	.year-wrap span {
		font-size: clamp(2rem, 3vw, 3.8rem);
		line-height: 0.95;
		letter-spacing: -0.05em;
		font-family: serif;
		color: white;
		word-break: break-word;
	}

	.year-wrap span.gold {
		color: #c5a059;
	}

	.right p {
		max-width: 30ch;
		font-size: 15px;
		line-height: 1.8;
		color: rgba(255,255,255,0.48);
	}

	@media (max-width: 1024px) {
		.comparison-row {
			grid-template-columns: 1fr;
			gap: 28px;
		}

		.right p {
			max-width: 100%;
		}
	}
</style>