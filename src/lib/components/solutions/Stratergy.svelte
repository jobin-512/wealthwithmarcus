<script lang="ts">
	import { onMount } from 'svelte';

	let sectionRef: HTMLElement;
	let timelineRef: HTMLElement[] = [];

	const strategySteps = [
		{
			number: '01',
			title: 'Most Homeowners Are Trapped',
			text:
				'Millions locked into 2–4% mortgage rates feel financially stuck. Refinancing destroys their existing position, so they sit on equity they can’t efficiently use.',
			highlight: 'The rate isn’t the problem.'
		},
		{
			number: '02',
			title: 'Refinancing Solves The Wrong Problem',
			text:
				'A refinance resets amortization, increases long-term interest exposure, and often creates more debt drag — even when the monthly payment looks lower.',
			highlight: 'Lower payments ≠ optimized cash flow.'
		},
		{
			number: '03',
			title: 'First Lien HELOC Changes The Math',
			text:
				'Instead of replacing your mortgage, income flows directly against principal daily. This reduces interest exposure faster while maintaining liquidity and flexibility.',
			highlight: 'Your money starts working immediately.'
		},
		{
			number: '04',
			title: 'Control Creates Acceleration',
			text:
				'The goal is not “cheap debt.” The goal is control over how cash moves. Strategic equity management can compress payoff timelines dramatically.',
			highlight: 'Cash flow becomes a financial weapon.'
		}
	];

	onMount(() => {
		let cleanup: (() => void) | undefined;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			const ctx = gsap.context(() => {
				gsap.from(timelineRef, {
					opacity: 0,
					y: 120,
					stagger: 0.18,
					duration: 1.2,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 72%'
					}
				});

				timelineRef.forEach((card) => {
					card?.addEventListener('mousemove', (e: MouseEvent) => {
						const rect = card.getBoundingClientRect();

						const x = e.clientX - rect.left;
						const y = e.clientY - rect.top;

						gsap.to(card, {
							rotateY: gsap.utils.mapRange(0, rect.width, -7, 7, x),
							rotateX: gsap.utils.mapRange(0, rect.height, 7, -7, y),
							transformPerspective: 1200,
							duration: 0.45,
							ease: 'power3.out'
						});
					});

					card?.addEventListener('mouseleave', () => {
						gsap.to(card, {
							rotateX: 0,
							rotateY: 0,
							duration: 1,
							ease: 'elastic.out(1,0.45)'
						});
					});
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
	class="relative overflow-hidden bg-[var(--theme-color)] py-32 lg:py-44"
>
	<!-- Background -->
	<div class="noise"></div>

	<div class="absolute inset-0">
		<div class="gradient-orb orb-left"></div>
		<div class="gradient-orb orb-right"></div>
	</div>

	<div class="vignette"></div>

	<!-- Header -->
	<div class="relative z-10 mx-auto mb-24 max-w-7xl px-6">
		<div class="max-w-4xl">
			<div
				class="mb-7 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl"
			>
				<div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

				<span class="font-mono text-[11px] uppercase tracking-[0.32em] text-white/55">
					The Strategy Most Banks Never Explain
				</span>
			</div>

			<h2
				class="font-serif text-[clamp(3rem,6vw,6.2rem)] leading-[0.92] tracking-[-0.05em] text-white"
			>
				The Problem
				<br />
				<span class="italic text-[#c5a059]">Isn’t Your Mortgage.</span>
			</h2>

			<p class="mt-10 max-w-3xl text-lg leading-relaxed text-white/55 lg:text-xl">
				Most homeowners focus on interest rates.
				<br />
				High-level financial strategy focuses on how money flows.
			</p>
		</div>
	</div>

	<!-- Timeline -->
	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<div class="timeline-line"></div>

		<div class="space-y-10">
			{#each strategySteps as step, i}
				<div
					bind:this={timelineRef[i]}
					class="strategy-card"
				>
					<!-- Glow -->
					<div class="card-glow"></div>

					<!-- Shine -->
					<div class="card-shine"></div>

					<div class="relative z-10 grid gap-10 lg:grid-cols-[140px_1fr] lg:gap-16">
						<!-- Number -->
						<div>
							<div class="step-number">
								{step.number}
							</div>
						</div>

						<!-- Content -->
						<div>
							<h3
								class="mb-5 font-serif text-[2rem] leading-[1.05] tracking-[-0.03em] text-white lg:text-[2.6rem]"
							>
								{step.title}
							</h3>

							<p
								class="max-w-3xl text-[15px] leading-relaxed text-white/55 lg:text-[17px]"
							>
								{step.text}
							</p>

							<div class="highlight-box">
								<div class="highlight-dot"></div>

								<p>{step.highlight}</p>
							</div>
						</div>
					</div>
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
		pointer-events: none;
		background-image:
			radial-gradient(rgba(255,255,255,0.35) 0.5px, transparent 0.5px);
		background-size: 4px 4px;
	}

	.vignette {
		position: absolute;
		inset: 0;

		background:
			radial-gradient(circle at center,
			transparent 20%,
			rgba(0,0,0,0.82) 100%);
	}

	.gradient-orb {
		position: absolute;
		border-radius: 9999px;
		filter: blur(140px);
	}

	.orb-left {
		left: -250px;
		top: -250px;
		width: 700px;
		height: 700px;

		background: rgba(197,160,89,0.1);
	}

	.orb-right {
		right: -300px;
		bottom: -300px;
		width: 850px;
		height: 850px;

		background: rgba(197,160,89,0.08);
	}

	.timeline-line {
		position: absolute;
		left: 85px;
		top: 0;
		bottom: 0;

		width: 1px;

		background:
			linear-gradient(
				to bottom,
				transparent,
				rgba(197,160,89,0.4),
				transparent
			);
	}

	.strategy-card {
		position: relative;
		overflow: hidden;

		padding: 42px;

		border-radius: 36px;

		border: 1px solid rgba(255,255,255,0.08);

		background: rgba(255,255,255,0.03);

		backdrop-filter: blur(24px);

		transition:
			transform 0.5s ease,
			border-color 0.5s ease,
			box-shadow 0.5s ease;
	}

	.strategy-card:hover {
		border-color: rgba(197,160,89,0.35);

		box-shadow:
			0 0 90px rgba(197,160,89,0.08);
	}

	.card-glow {
		position: absolute;
		inset: 0;

		background:
			radial-gradient(circle at top,
			rgba(197,160,89,0.15),
			transparent 70%);

		opacity: 0;

		transition: opacity 0.5s ease;
	}

	.strategy-card:hover .card-glow {
		opacity: 1;
	}

	.card-shine {
		position: absolute;
		inset: 0;

		background:
			linear-gradient(
				115deg,
				transparent 40%,
				rgba(255,255,255,0.04) 50%,
				transparent 60%
			);

		transform: translateX(-120%) rotate(12deg);

		animation: shineMove 10s linear infinite;
	}

	.step-number {
		position: relative;

		width: 92px;
		height: 92px;

		display: flex;
		align-items: center;
		justify-content: center;

		border-radius: 9999px;

		border: 1px solid rgba(197,160,89,0.28);

		background:
			radial-gradient(circle at top,
			rgba(197,160,89,0.15),
			rgba(255,255,255,0.03));

		font-family: serif;
		font-size: 2rem;
		color: #c5a059;

		box-shadow:
			0 0 40px rgba(197,160,89,0.08);
	}

	.highlight-box {
		margin-top: 32px;

		display: inline-flex;
		align-items: center;
		gap: 14px;

		padding: 16px 22px;

		border-radius: 9999px;

		border: 1px solid rgba(197,160,89,0.18);

		background: rgba(197,160,89,0.06);
	}

	.highlight-dot {
		width: 8px;
		height: 8px;

		border-radius: 9999px;

		background: #c5a059;

		box-shadow:
			0 0 20px rgba(197,160,89,0.7);
	}

	.highlight-box p {
		font-size: 13px;
		letter-spacing: 0.18em;
		text-transform: uppercase;
		color: rgba(255,255,255,0.75);
	}

	@keyframes shineMove {
		from {
			transform: translateX(-120%) rotate(12deg);
		}

		to {
			transform: translateX(120%) rotate(12deg);
		}
	}

	@media (max-width: 1024px) {
		.timeline-line {
			display: none;
		}
	}
</style>