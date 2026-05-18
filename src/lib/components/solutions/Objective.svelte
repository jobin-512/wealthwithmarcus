<script lang="ts">
	import { onMount } from 'svelte';

	let sectionRef: HTMLElement;
	let titleRef: HTMLElement;
	let metricRefs: HTMLElement[] = $state([]);
	let centerTextRef: HTMLElement;

	const metrics = [
		{
			value: '$250M+',
			label: 'Structured Capital Solutions'
		},
		{
			value: '5–7 Years',
			label: 'Accelerated Payoff Strategies'
		},
		{
			value: '24hr',
			label: 'Fast-Track Digital Reviews'
		},
		{
			value: '100%',
			label: 'Strategy-First Consultation'
		}
	];

	onMount(() => {
		let cleanup: (() => void) | undefined;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			const ctx = gsap.context(() => {
				gsap.from(titleRef, {
					y: 80,
					opacity: 0,
					duration: 1.2,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: titleRef,
						start: 'top 85%'
					}
				});

				gsap.from(centerTextRef, {
					scale: 0.9,
					opacity: 0,
					duration: 1.4,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: centerTextRef,
						start: 'top 80%'
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

			cleanup = () => {
				ctx.revert();
				ScrollTrigger.getAll().forEach((t) => t.kill());
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
	class="relative overflow-hidden bg-[var(--theme-color)] py-32 lg:py-52"
>
	<!-- background -->
	<div class="noise"></div>

	<div class="orb orb-1"></div>
	<div class="orb orb-2"></div>

	<div class="grid-lines"></div>

	<div class="vignette"></div>

	<!-- top border -->
	<div
		class="absolute inset-x-0 top-0 h-px bg-gradient-to-r from-transparent via-white/10 to-transparent"
	></div>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<!-- heading -->
		<div
			bind:this={titleRef}
			class="mx-auto max-w-5xl text-center"
		>
			<div class="eyebrow">
				<div class="dot"></div>

				<span>Financial Positioning</span>
			</div>

			<h2
				class="mt-10 font-serif text-[clamp(3.5rem,8vw,8rem)] leading-[0.9] tracking-[-0.06em] text-white"
			>
				Most People
				<br />
				Optimize For Rates.
			</h2>
		</div>

		<!-- center statement -->
		<div
			bind:this={centerTextRef}
			class="center-piece"
		>
			<div class="circle circle-1"></div>
			<div class="circle circle-2"></div>
			<div class="circle circle-3"></div>

			<div class="center-content">
				<p class="small-label">
					REAL OBJECTIVE
				</p>

				<h3>
					Optimize
					<br />
					For Control.
				</h3>

				<p class="desc">
					The goal is not simply obtaining financing.
					The goal is structuring money, debt, and equity
					in a way that creates flexibility, liquidity,
					and long-term leverage.
				</p>
			</div>
		</div>

		<!-- metrics -->
		<div class="metrics-grid">
			{#each metrics as metric, i}
				<div
					bind:this={metricRefs[i]}
					class="metric"
				>
					<div class="metric-line"></div>

					<h4>{metric.value}</h4>

					<p>{metric.label}</p>
				</div>
			{/each}
		</div>

		<!-- quote -->
		<div class="quote-wrap">
			<div class="quote-line"></div>

			<p>
				“Your mortgage should support your financial strategy.
				<br />
				Not trap your liquidity.”
			</p>
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
			radial-gradient(rgba(255,255,255,0.35) 0.5px, transparent 0.5px);

		background-size: 4px 4px;
	}

	.grid-lines {
		position: absolute;
		inset: 0;

		background-image:
			linear-gradient(rgba(255,255,255,0.03) 1px, transparent 1px),
			linear-gradient(90deg, rgba(255,255,255,0.03) 1px, transparent 1px);

		background-size: 120px 120px;

		mask-image:
			radial-gradient(circle at center,
			black 20%,
			transparent 85%);
	}

	.vignette {
		position: absolute;
		inset: 0;

		background:
			radial-gradient(circle at center,
			transparent 18%,
			rgba(0,0,0,0.85) 100%);
	}

	.orb {
		position: absolute;
		border-radius: 9999px;
		filter: blur(160px);
	}

	.orb-1 {
		top: -250px;
		left: -250px;

		width: 800px;
		height: 800px;

		background: rgba(197,160,89,0.09);
	}

	.orb-2 {
		right: -250px;
		bottom: -250px;

		width: 700px;
		height: 700px;

		background: rgba(197,160,89,0.06);
	}

	.eyebrow {
		display: inline-flex;
		align-items: center;
		gap: 14px;

		padding: 12px 20px;

		border-radius: 9999px;

		border: 1px solid rgba(255,255,255,0.08);

		background: rgba(255,255,255,0.03);

		backdrop-filter: blur(20px);
	}

	.eyebrow span {
		font-size: 11px;
		letter-spacing: 0.32em;
		text-transform: uppercase;
		font-family: monospace;

		color: rgba(255,255,255,0.55);
	}

	.dot {
		width: 8px;
		height: 8px;
		border-radius: 9999px;

		background: #c5a059;

		box-shadow:
			0 0 18px rgba(197,160,89,0.8);
	}

	.center-piece {
		position: relative;

		display: flex;
		align-items: center;
		justify-content: center;

		margin-top: 120px;
		min-height: 650px;
	}

	.circle {
		position: absolute;
		border-radius: 9999px;
		border: 1px solid rgba(255,255,255,0.06);
	}

	.circle-1 {
		width: 620px;
		height: 620px;

		animation: rotateSlow 40s linear infinite;
	}

	.circle-2 {
		width: 440px;
		height: 440px;

		border-style: dashed;

		animation: rotateReverse 30s linear infinite;
	}

	.circle-3 {
		width: 260px;
		height: 260px;

		background:
			radial-gradient(circle at center,
			rgba(197,160,89,0.12),
			transparent 70%);
	}

	.center-content {
		position: relative;
		z-index: 5;

		max-width: 620px;

		text-align: center;
	}

	.small-label {
		margin-bottom: 22px;

		font-size: 10px;
		letter-spacing: 0.32em;
		text-transform: uppercase;
		font-family: monospace;

		color: rgba(255,255,255,0.35);
	}

	.center-content h3 {
		font-size: clamp(3rem, 7vw, 7rem);
		line-height: 0.92;
		letter-spacing: -0.06em;
		font-family: serif;

		color: white;
	}

	.desc {
		margin-top: 34px;

		font-size: 18px;
		line-height: 1.9;

		color: rgba(255,255,255,0.5);
	}

	.metrics-grid {
		display: grid;
		grid-template-columns: repeat(4, 1fr);
		gap: 30px;

		margin-top: 80px;
	}

	.metric {
		position: relative;

		padding-top: 30px;
	}

	.metric-line {
		position: absolute;
		left: 0;
		top: 0;

		width: 100%;
		height: 1px;

		background:
			linear-gradient(
				to right,
				rgba(197,160,89,0.7),
				transparent
			);
	}

	.metric h4 {
		font-size: clamp(2rem, 4vw, 4rem);
		line-height: 1;
		letter-spacing: -0.05em;
		font-family: serif;

		color: white;
	}

	.metric p {
		margin-top: 16px;

		font-size: 14px;
		line-height: 1.7;

		color: rgba(255,255,255,0.45);
	}

	.quote-wrap {
		margin-top: 160px;

		text-align: center;
	}

	.quote-line {
		width: 1px;
		height: 120px;

		margin-inline: auto;
		margin-bottom: 40px;

		background:
			linear-gradient(
				to bottom,
				transparent,
				rgba(197,160,89,0.8),
				transparent
			);
	}

	.quote-wrap p {
		font-size: clamp(2rem, 4vw, 4rem);
		line-height: 1.15;
		letter-spacing: -0.04em;
		font-family: serif;

		color: rgba(255,255,255,0.92);
	}

	@keyframes rotateSlow {
		from {
			transform: rotate(0deg);
		}

		to {
			transform: rotate(360deg);
		}
	}

	@keyframes rotateReverse {
		from {
			transform: rotate(360deg);
		}

		to {
			transform: rotate(0deg);
		}
	}

	@media (max-width: 1024px) {
		.metrics-grid {
			grid-template-columns: repeat(2, 1fr);
		}

		.center-piece {
			min-height: 520px;
		}

		.circle-1 {
			width: 460px;
			height: 460px;
		}

		.circle-2 {
			width: 320px;
			height: 320px;
		}
	}

	@media (max-width: 768px) {
		.metrics-grid {
			grid-template-columns: 1fr;
		}

		.center-piece {
			min-height: 420px;
			margin-top: 90px;
		}

		.circle-1 {
			width: 320px;
			height: 320px;
		}

		.circle-2 {
			width: 220px;
			height: 220px;
		}

		.circle-3 {
			width: 140px;
			height: 140px;
		}

		.desc {
			font-size: 16px;
		}
	}
</style>