<script lang="ts">
	import { onMount } from 'svelte';

	let sectionRef: HTMLElement;
	let spotlightRef: HTMLElement;
	let headlineRef: HTMLElement;

	let mathLines: HTMLElement[] = [];
	let revealItems: HTMLElement[] = [];

	const comparisons = [
		{
			title: 'Traditional Thinking',
			text: 'Most homeowners obsess over the interest rate while ignoring the actual cash-flow structure of the loan.'
		},
		{
			title: 'What Actually Matters',
			text: 'The speed at which principal reduces determines how much interest you truly pay over time.'
		}
	];

	const statFlow = [
		{
			label: 'Mortgage Payment',
			value: '$4,200/mo'
		},
		{
			label: 'Interest-Heavy Years',
			value: '10–15 yrs'
		},
		{
			label: 'Cash Flow Locked',
			value: '100%'
		}
	];

	onMount(() => {
		let cleanup: (() => void) | undefined;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			const xTo = gsap.quickTo(spotlightRef, 'x', {
				duration: 1,
				ease: 'power3.out'
			});

			const yTo = gsap.quickTo(spotlightRef, 'y', {
				duration: 1,
				ease: 'power3.out'
			});

			const moveSpotlight = (e: MouseEvent) => {
				const rect = sectionRef.getBoundingClientRect();

				xTo(e.clientX - rect.left - 250);
				yTo(e.clientY - rect.top - 250);
			};

			sectionRef.addEventListener('mousemove', moveSpotlight);

			const ctx = gsap.context(() => {
				gsap.from(headlineRef.querySelectorAll('.split'), {
					y: 120,
					opacity: 0,
					rotateX: -90,
					stagger: 0.05,
					duration: 1.2,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: headlineRef,
						start: 'top 80%'
					}
				});

				gsap.from(revealItems.filter(Boolean), {
					y: 80,
					opacity: 0,
					stagger: 0.12,
					duration: 1,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 70%'
					}
				});

				gsap.from(mathLines.filter(Boolean), {
					scaleX: 0,
					opacity: 0,
					stagger: 0.15,
					duration: 1.4,
					transformOrigin: 'left',
					ease: 'power4.out',
					scrollTrigger: {
						trigger: mathLines[0],
						start: 'top 85%'
					}
				});
			}, sectionRef);

			cleanup = () => {
				ctx.revert();
				sectionRef?.removeEventListener('mousemove', moveSpotlight);
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
	class="relative overflow-hidden bg-[var(--theme-color)] py-32 lg:py-44"
>
	<!-- BACKGROUND -->
	<div class="noise"></div>

	<div class="mesh mesh-1"></div>
	<div class="mesh mesh-2"></div>

	<div class="vignette"></div>

	<!-- SPOTLIGHT -->
	<div
		bind:this={spotlightRef}
		class="spotlight"
	></div>

	<!-- GOLD LINE -->
	<svg
		class="pointer-events-none absolute inset-0 h-full w-full opacity-[0.06]"
		preserveAspectRatio="none"
	>
		<defs>
			<linearGradient id="gold-flow-2" x1="0%" y1="0%" x2="100%" y2="0%">
				<stop offset="0%" stop-color="#c5a059" stop-opacity="0" />
				<stop offset="50%" stop-color="#c5a059" stop-opacity="1" />
				<stop offset="100%" stop-color="#c5a059" stop-opacity="0" />
			</linearGradient>
		</defs>

		<path
			d="M0,200 C450,50 800,300 1200,120 S1700,80 1920,200"
			stroke="url(#gold-flow-2)"
			stroke-width="1"
			fill="none"
		>
			<animate
				attributeName="d"
				dur="18s"
				repeatCount="indefinite"
				values="
        M0,200 C450,50 800,300 1200,120 S1700,80 1920,200;
        M0,150 C350,260 900,40 1200,200 S1700,260 1920,140;
        M0,200 C450,50 800,300 1200,120 S1700,80 1920,200"
			/>
		</path>
	</svg>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<!-- HEADER -->
		<div class="max-w-5xl">
			<div class="eyebrow">
				<div class="dot"></div>

				<span>
					The Mortgage Industry Never Explains This
				</span>
			</div>

			<h2
				bind:this={headlineRef}
				class="headline"
			>
				<span class="split">The</span>
				<span class="split accent">Interest</span>
				<span class="split accent">Rate</span>
				<span class="split">Isn’t</span>

				<br class="hidden lg:block" />

				<span class="split">The</span>
				<span class="split">Real</span>
				<span class="split accent">Problem.</span>
			</h2>

			<p class="intro">
				Most homeowners are trapped because their mortgage structure forces
				them to stay interest-heavy for decades.
				The system rewards slow principal reduction —
				not financial efficiency.
			</p>
		</div>

		<!-- MAIN -->
		<div class="mt-24 grid gap-20 lg:grid-cols-[1fr_0.9fr] lg:items-start">
			<!-- LEFT -->
			<div class="space-y-16">
				{#each comparisons as item, i}
					<div
						bind:this={revealItems[i]}
						class="comparison-row"
					>
						<div class="comparison-number">
							0{i + 1}
						</div>

						<div class="comparison-content">
							<h3>
								{item.title}
							</h3>

							<p>
								{item.text}
							</p>
						</div>
					</div>
				{/each}

				<!-- MATH FLOW -->
				<div class="math-block">
					<div class="math-top">
						<p>
							A 30-year mortgage is front-loaded with interest.
						</p>

						<p class="muted">
							Your payment stays the same —
							but your principal barely moves in the early years.
						</p>
					</div>

					<div class="math-visual">
						<div bind:this={mathLines[0]} class="math-line gold">
							<span>Interest</span>
							<div class="bar w-[88%]"></div>
						</div>

						<div bind:this={mathLines[1]} class="math-line white">
							<span>Principal</span>
							<div class="bar w-[18%]"></div>
						</div>
					</div>
				</div>
			</div>

			<!-- RIGHT -->
			<div
				bind:this={revealItems[2]}
				class="insight-panel"
			>
				<div class="panel-glow"></div>

				<div class="relative z-10">
					<div class="panel-tag">
						Cash Flow Reality
					</div>

					<div class="space-y-8">
						{#each statFlow as stat}
							<div class="stat-row">
								<div class="stat-label">
									{stat.label}
								</div>

								<div class="stat-value">
									{stat.value}
								</div>
							</div>
						{/each}
					</div>

					<div class="divider"></div>

					<p class="panel-copy">
						This is why refinancing alone often fails.
						You may lower the rate —
						but restart the interest cycle all over again.
					</p>

					<div class="quote">
						“Lower payment” and “faster payoff”
						are not the same thing.
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
		background-image:
			radial-gradient(rgba(255,255,255,0.35) 0.5px, transparent 0.5px);
		background-size: 4px 4px;
		pointer-events: none;
	}

	.mesh {
		position: absolute;
		border-radius: 9999px;
		filter: blur(120px);
	}

	.mesh-1 {
		top: -240px;
		left: -200px;
		width: 700px;
		height: 700px;
		background: rgba(197,160,89,0.12);
	}

	.mesh-2 {
		right: -300px;
		bottom: -260px;
		width: 800px;
		height: 800px;
		background: rgba(197,160,89,0.08);
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

	.spotlight {
		position: absolute;
		left: 0;
		top: 0;
		width: 500px;
		height: 500px;
		border-radius: 9999px;
		background: rgba(197,160,89,0.08);
		filter: blur(120px);
		pointer-events: none;
	}

	.eyebrow {
		display: inline-flex;
		align-items: center;
		gap: 14px;

		padding: 14px 22px;

		border-radius: 9999px;

		border: 1px solid rgba(255,255,255,0.08);

		background: rgba(255,255,255,0.03);

		backdrop-filter: blur(14px);
	}

	.dot {
		width: 8px;
		height: 8px;
		border-radius: 9999px;
		background: #c5a059;
		box-shadow: 0 0 18px #c5a059;
	}

	.eyebrow span {
		font-size: 11px;
		letter-spacing: 0.32em;
		text-transform: uppercase;
		color: rgba(255,255,255,0.52);
		font-family: monospace;
	}

	.headline {
		margin-top: 32px;

		font-family: serif;

		font-size: clamp(3rem, 6vw, 6.5rem);

		line-height: 0.95;

		letter-spacing: -0.05em;

		color: white;
	}

	.headline .split {
		display: inline-block;
		margin-right: 0.2em;
	}

	.accent {
		color: #c5a059;
		font-style: italic;
	}

	.intro {
		margin-top: 34px;

		max-width: 860px;

		font-size: 1.2rem;
		line-height: 1.9;

		color: rgba(255,255,255,0.58);
	}

	.comparison-row {
		display: grid;
		grid-template-columns: 90px 1fr;
		gap: 30px;
		align-items: start;
	}

	.comparison-number {
		font-size: clamp(3rem, 5vw, 5rem);
		font-family: serif;
		line-height: 1;
		color: rgba(255,255,255,0.08);
	}

	.comparison-content h3 {
		font-size: clamp(1.5rem, 2vw, 2.2rem);
		color: white;
		letter-spacing: -0.03em;
		margin-bottom: 16px;
	}

	.comparison-content p {
		max-width: 640px;
		font-size: 1.02rem;
		line-height: 1.9;
		color: rgba(255,255,255,0.56);
	}

	.math-block {
		padding-top: 12px;
	}

	.math-top p {
		font-size: 1rem;
		line-height: 1.8;
		color: rgba(255,255,255,0.82);
	}

	.math-top .muted {
		margin-top: 10px;
		color: rgba(255,255,255,0.46);
	}

	.math-visual {
		margin-top: 38px;
		display: flex;
		flex-direction: column;
		gap: 24px;
	}

	.math-line span {
		display: block;
		margin-bottom: 10px;

		font-size: 11px;
		letter-spacing: 0.3em;
		text-transform: uppercase;

		color: rgba(255,255,255,0.4);

		font-family: monospace;
	}

	.bar {
		height: 12px;
		border-radius: 9999px;
	}

	.gold .bar {
		background: linear-gradient(
			90deg,
			#c5a059,
			rgba(197,160,89,0.2)
		);
		box-shadow: 0 0 30px rgba(197,160,89,0.25);
	}

	.white .bar {
		background: linear-gradient(
			90deg,
			white,
			rgba(255,255,255,0.1)
		);
	}

	.insight-panel {
		position: relative;
		overflow: hidden;

		padding: 42px;

		border-radius: 38px;

		border: 1px solid rgba(255,255,255,0.08);

		background: rgba(255,255,255,0.03);

		backdrop-filter: blur(24px);
	}

	.panel-glow {
		position: absolute;
		inset: 0;

		background:
			radial-gradient(circle at top,
			rgba(197,160,89,0.14),
			transparent 70%);
	}

	.panel-tag {
		display: inline-flex;

		padding: 10px 16px;

		border-radius: 9999px;

		background: rgba(255,255,255,0.04);

		border: 1px solid rgba(255,255,255,0.08);

		font-size: 10px;
		letter-spacing: 0.28em;
		text-transform: uppercase;

		color: rgba(255,255,255,0.5);

		font-family: monospace;
	}

	.stat-row {
		display: flex;
		align-items: end;
		justify-content: space-between;
		gap: 20px;

		padding-bottom: 18px;

		border-bottom: 1px solid rgba(255,255,255,0.06);
	}

	.stat-label {
		font-size: 13px;
		letter-spacing: 0.18em;
		text-transform: uppercase;
		color: rgba(255,255,255,0.42);
		font-family: monospace;
	}

	.stat-value {
		font-size: clamp(1.8rem, 3vw, 3rem);
		font-family: serif;
		line-height: 1;
		color: white;
	}

	.divider {
		height: 1px;
		margin: 40px 0;
		background: linear-gradient(
			90deg,
			transparent,
			rgba(255,255,255,0.1),
			transparent
		);
	}

	.panel-copy {
		font-size: 1rem;
		line-height: 1.9;
		color: rgba(255,255,255,0.58);
	}

	.quote {
		margin-top: 36px;

		padding-left: 22px;

		border-left: 2px solid #c5a059;

		font-size: 1.3rem;
		line-height: 1.7;

		color: white;
	}

	@media (max-width: 768px) {
		.comparison-row {
			grid-template-columns: 1fr;
			gap: 12px;
		}

		.comparison-number {
			font-size: 3rem;
		}

		.insight-panel {
			padding: 28px;
		}
	}
</style>