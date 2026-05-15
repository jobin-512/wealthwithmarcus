<script lang="ts">
	import { onMount } from 'svelte';

	let sectionRef: HTMLElement;
	let titleRef: HTMLElement;
	let leftRef: HTMLElement;
	let rightRef: HTMLElement;
	let metricRefs: HTMLElement[] = [];

	const metrics = [
		{
			value: '$487K',
			label: 'Interest Avoided',
			desc: 'Compared to maintaining a traditional 30-year structure.'
		},
		{
			value: '7 Years',
			label: 'Projected Payoff',
			desc: 'Using cash-flow optimized equity velocity strategies.'
		},
		{
			value: '$4.2M',
			label: 'Equity Preserved',
			desc: 'Without forcing liquidation or refinancing existing assets.'
		}
	];

	onMount(() => {
		let cleanup: (() => void) | undefined;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			const ctx = gsap.context(() => {
				gsap.from(titleRef.children, {
					y: 80,
					opacity: 0,
					rotateX: -70,
					stagger: 0.05,
					duration: 1.1,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: titleRef,
						start: 'top 84%'
					}
				});

				gsap.from(leftRef, {
					x: -80,
					opacity: 0,
					duration: 1.2,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: leftRef,
						start: 'top 82%'
					}
				});

				gsap.from(rightRef, {
					x: 80,
					opacity: 0,
					duration: 1.2,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: rightRef,
						start: 'top 82%'
					}
				});

				gsap.from(metricRefs, {
					y: 60,
					opacity: 0,
					stagger: 0.12,
					duration: 1,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: metricRefs[0],
						start: 'top 88%'
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
	class="relative overflow-hidden bg-[#050505] py-32 lg:py-44"
>
	<!-- background -->
	<div class="noise"></div>

	<div
		class="absolute left-[-10%] top-[0%] h-[700px] w-[700px] rounded-full bg-[#c5a059]/[0.06] blur-[150px]"
	></div>

	<div
		class="absolute bottom-[-20%] right-[-10%] h-[800px] w-[800px] rounded-full bg-[#c5a059]/[0.05] blur-[180px]"
	></div>

	<div class="vignette"></div>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<!-- heading -->
		<div class="mb-24 max-w-5xl">
			<div
				class="mb-7 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl"
			>
				<div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

				<span class="font-mono text-[11px] uppercase tracking-[0.32em] text-white/50">
					Scenario Modeling
				</span>
			</div>

			<h2
				bind:this={titleRef}
				class="font-serif text-[clamp(3rem,6vw,6.5rem)] leading-[0.92] tracking-[-0.05em] text-white"
				style="perspective:1000px;"
			>
				<span class="mr-[0.16em] inline-block">This</span>
				<span class="mr-[0.16em] inline-block">isn’t</span>
				<span class="mr-[0.16em] inline-block italic text-[#c5a059]">
					theoretical.
				</span>

				<br />

				<span class="mr-[0.16em] inline-block">It’s</span>
				<span class="mr-[0.16em] inline-block">just</span>
				<span class="mr-[0.16em] inline-block">different</span>
				<span class="mr-[0.16em] inline-block text-[#c5a059]">
					math.
				</span>
			</h2>
		</div>

		<!-- comparison -->
		<div class="grid gap-16 lg:grid-cols-[1fr_auto_1fr] lg:gap-14">
			<!-- left -->
			<div bind:this={leftRef}>
				<div class="side-label">
					Traditional Structure
				</div>

				<div class="timeline">
					<div class="year">
						<span>Year 1</span>
						<div class="bar long"></div>
					</div>

					<div class="year">
						<span>Year 10</span>
						<div class="bar medium"></div>
					</div>

					<div class="year">
						<span>Year 20</span>
						<div class="bar short"></div>
					</div>

					<div class="year">
						<span>Year 30</span>
						<div class="bar tiny"></div>
					</div>
				</div>

				<p class="bottom-copy">
					Most of the early payments go toward interest —
					not principal reduction.
				</p>
			</div>

			<!-- center -->
			<div class="center-line-wrap">
				<div class="center-line"></div>

				<div class="center-dot"></div>
			</div>

			<!-- right -->
			<div bind:this={rightRef}>
				<div class="side-label gold">
					Strategic Structure
				</div>

				<div class="timeline">
					<div class="year">
						<span>Year 1</span>
						<div class="bar gold medium"></div>
					</div>

					<div class="year">
						<span>Year 3</span>
						<div class="bar gold short"></div>
					</div>

					<div class="year">
						<span>Year 5</span>
						<div class="bar gold tiny"></div>
					</div>

					<div class="year">
						<span>Year 7</span>
						<div class="bar gold micro"></div>
					</div>
				</div>

				<p class="bottom-copy">
					Cash flow continuously attacks principal —
					reducing the total interest burden dramatically faster.
				</p>
			</div>
		</div>

		<!-- metrics -->
		<div class="mt-28 grid gap-10 border-t border-white/10 pt-16 lg:grid-cols-3">
			{#each metrics as item, i}
				<div
					bind:this={metricRefs[i]}
					class="metric"
				>
					<div class="metric-value">
						{item.value}
					</div>

					<div class="metric-label">
						{item.label}
					</div>

					<p>
						{item.desc}
					</p>
				</div>
			{/each}
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
		background-image: radial-gradient(rgba(255,255,255,0.35) 0.5px, transparent 0.5px);
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
			rgba(0,0,0,0.84) 100%);
	}

	.side-label {
		margin-bottom: 40px;

		font-size: 11px;
		letter-spacing: 0.28em;
		text-transform: uppercase;
		color: rgba(255,255,255,0.35);
		font-family: monospace;
	}

	.side-label.gold {
		color: #c5a059;
	}

	.timeline {
		display: flex;
		flex-direction: column;
		gap: 34px;
	}

	.year {
		display: grid;
		grid-template-columns: 90px 1fr;
		align-items: center;
		gap: 28px;
	}

	.year span {
		font-size: 15px;
		color: rgba(255,255,255,0.65);
	}

	.bar {
		height: 1px;
		background: rgba(255,255,255,0.75);
	}

	.bar.gold {
		background: linear-gradient(
			90deg,
			#c5a059,
			rgba(255,255,255,0.9)
		);
	}

	.long {
		width: 100%;
	}

	.medium {
		width: 72%;
	}

	.short {
		width: 45%;
	}

	.tiny {
		width: 20%;
	}

	.micro {
		width: 8%;
	}

	.center-line-wrap {
		position: relative;
		display: flex;
		justify-content: center;
	}

	.center-line {
		width: 1px;
		background: rgba(255,255,255,0.1);
	}

	.center-dot {
		position: absolute;
		top: 50%;
		width: 12px;
		height: 12px;
		border-radius: 999px;
		background: #c5a059;
		box-shadow: 0 0 40px rgba(197,160,89,0.6);
		transform: translateY(-50%);
	}

	.bottom-copy {
		margin-top: 48px;
		max-width: 32ch;

		font-size: 16px;
		line-height: 1.8;
		color: rgba(255,255,255,0.48);
	}

	.metric {
		padding-right: 20px;
	}

	.metric-value {
		margin-bottom: 14px;

		font-size: clamp(2.5rem,4vw,5rem);
		line-height: 0.95;
		letter-spacing: -0.06em;
		color: #c5a059;
		font-family: serif;
	}

	.metric-label {
		margin-bottom: 14px;

		font-size: 12px;
		letter-spacing: 0.24em;
		text-transform: uppercase;
		color: rgba(255,255,255,0.34);
		font-family: monospace;
	}

	.metric p {
		max-width: 28ch;

		font-size: 15px;
		line-height: 1.8;
		color: rgba(255,255,255,0.5);
	}

	@media (max-width: 1024px) {
		.center-line-wrap {
			display: none;
		}

		.year {
			grid-template-columns: 70px 1fr;
		}
	}
</style>