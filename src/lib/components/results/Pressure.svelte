<script lang="ts">
	import { onMount } from 'svelte';
    import Pressure from "$lib/assets/results/6.webp"

	let sectionRef: HTMLElement;
	let imageWrap: HTMLElement;
	let timelineRef: HTMLElement;
	let quoteRef: HTMLElement;

	onMount(() => {
		let cleanup: (() => void) | undefined;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			const ctx = gsap.context(() => {
				// IMAGE
				gsap.from(imageWrap, {
					scale: 1.15,
					opacity: 0,
					duration: 1.5,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: imageWrap,
						start: 'top 82%'
					}
				});

				// TIMELINE
				gsap.from('.timeline-step', {
					y: 80,
					opacity: 0,
					duration: 1,
					stagger: 0.15,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: timelineRef,
						start: 'top 82%'
					}
				});

				// LINE GROW
				gsap.from('.timeline-line', {
					scaleY: 0,
					transformOrigin: 'top center',
					duration: 1.5,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: timelineRef,
						start: 'top 80%'
					}
				});

				// QUOTE
				gsap.from(quoteRef, {
					y: 80,
					opacity: 0,
					duration: 1.2,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: quoteRef,
						start: 'top 88%'
					}
				});
			}, sectionRef);

			cleanup = () => {
				ctx.revert();
			};
		};

		init();

		return () => cleanup?.();
	});
</script>

<section
	bind:this={sectionRef}
	class="relative overflow-hidden bg-[var(--theme-color)] py-32 lg:py-44"
>
	<!-- BG -->
	<div class="noise"></div>

	<div class="gradient-orb orb-1"></div>
	<div class="gradient-orb orb-2"></div>

	<div class="vignette"></div>

	<!-- TOP LINE -->
	<div
		class="absolute inset-x-0 top-0 h-px bg-gradient-to-r from-transparent via-white/10 to-transparent"
	></div>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<!-- HEADER -->
		<div class="mb-20 max-w-3xl">
			<p
				class="mb-6 font-mono text-[11px] uppercase tracking-[0.35em] text-[#c5a059]"
			>
				Real Transformation
			</p>

			<h2
				class="font-serif text-[clamp(3rem,5vw,5.5rem)] leading-[0.95] tracking-[-0.04em] text-white"
			>
				From Financial Pressure
				<span class="text-white/35">To Strategic Control.</span>
			</h2>
		</div>

		<div class="grid grid-cols-1 gap-20 lg:grid-cols-[1fr_0.95fr] lg:items-center">
			<!-- IMAGE -->
			<div bind:this={imageWrap} class="relative">
				<div class="image-frame">
					<img
						src={Pressure}
						alt="Luxury modern home"
						class="h-full w-full object-cover"
					/>
				</div>

				<!-- FLOATING STAT -->
				<div class="floating-card floating-1">
					<p class="label">Mortgage Timeline</p>
					<h3>30 Years → 7 Years</h3>
				</div>

				<div class="floating-card floating-2">
					<p class="label">Monthly Cash Flow</p>
					<h3>+$3,480</h3>
				</div>
			</div>

			<!-- TIMELINE -->
			<div bind:this={timelineRef} class="relative">
				<div class="timeline-line"></div>

				<div class="space-y-14">
					<div class="timeline-step">
						<div class="step-dot"></div>

						<div>
							<p class="step-label">Before</p>

							<h3 class="step-title">
								Locked Into A “Good Rate”
							</h3>

							<p class="step-desc">
								Client held a 3.1% mortgage and believed refinancing
								was the only way to access capital.
							</p>
						</div>
					</div>

					<div class="timeline-step">
						<div class="step-dot"></div>

						<div>
							<p class="step-label">Strategy Shift</p>

							<h3 class="step-title">
								Introduced First Lien HELOC Structure
							</h3>

							<p class="step-desc">
								Income redirected into the HELOC while maintaining
								liquidity and daily principal reduction.
							</p>
						</div>
					</div>

					<div class="timeline-step">
						<div class="step-dot"></div>

						<div>
							<p class="step-label">Outcome</p>

							<h3 class="step-title">
								Faster Payoff + More Cash Flow
							</h3>

							<p class="step-desc">
								Projected payoff timeline reduced from 30 years to
								under 7 years without refinancing.
							</p>
						</div>
					</div>
				</div>
			</div>
		</div>

		<!-- QUOTE -->
		<div
			bind:this={quoteRef}
			class="relative mt-28 overflow-hidden rounded-[38px] border border-white/10 bg-white/[0.03] p-10 backdrop-blur-2xl lg:p-16"
		>
			<div class="quote-glow"></div>

			<div class="relative z-10 max-w-5xl">
				<p
					class="mb-6 font-mono text-[11px] uppercase tracking-[0.35em] text-[#c5a059]"
				>
					Client Perspective
				</p>

				<h3
					class="font-serif text-[clamp(2rem,4vw,4rem)] leading-[1.08] tracking-[-0.03em] text-white"
				>
					“We stopped thinking like borrowers...
					<span class="text-white/35">
						and started thinking like capital managers.”
					</span>
				</h3>

				<p
					class="mt-8 max-w-3xl text-lg leading-relaxed text-white/55"
				>
					The biggest transformation wasn’t the lower interest paid.
					It was the psychological shift from reacting to debt...
					to controlling cash flow strategically.
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
		opacity: 0.025;
		background-image:
			radial-gradient(rgba(255,255,255,0.35) 0.5px, transparent 0.5px);
		background-size: 4px 4px;
		pointer-events: none;
	}

	.vignette {
		position: absolute;
		inset: 0;

		background:
			radial-gradient(circle at center,
			transparent 20%,
			rgba(0,0,0,0.8) 100%);
	}

	.gradient-orb {
		position: absolute;
		border-radius: 9999px;
		filter: blur(140px);
	}

	.orb-1 {
		top: -220px;
		left: -200px;

		width: 700px;
		height: 700px;

		background: rgba(197,160,89,0.12);
	}

	.orb-2 {
		right: -240px;
		bottom: -260px;

		width: 800px;
		height: 800px;

		background: rgba(197,160,89,0.08);
	}

	.image-frame {
		position: relative;
		overflow: hidden;

		border-radius: 36px;
		border: 1px solid rgba(255,255,255,0.08);

		height: 720px;
	}

	.image-frame::after {
		content: '';

		position: absolute;
		inset: 0;

		background:
			linear-gradient(
				to top,
				rgba(0,0,0,0.5),
				transparent 30%
			);
	}

	.floating-card {
		position: absolute;

		padding: 22px 24px;

		border-radius: 24px;

		border: 1px solid rgba(255,255,255,0.08);

		background: rgba(0,0,0,0.55);

		backdrop-filter: blur(24px);

		box-shadow:
			0 20px 60px rgba(0,0,0,0.45);
	}

	.floating-1 {
		left: -30px;
		top: 60px;
	}

	.floating-2 {
		right: -30px;
		bottom: 60px;
	}

	.label {
		margin-bottom: 8px;

		font-size: 10px;
		letter-spacing: 0.25em;
		text-transform: uppercase;

		color: rgba(255,255,255,0.35);
	}

	.floating-card h3 {
		font-size: 26px;
		font-weight: 500;
		color: white;
	}

	.timeline-line {
		position: absolute;
		left: 17px;
		top: 10px;
		bottom: 10px;

		width: 1px;

		background:
			linear-gradient(
				to bottom,
				rgba(197,160,89,0.8),
				rgba(255,255,255,0.08)
			);
	}

	.timeline-step {
		position: relative;

		padding-left: 70px;
	}

	.step-dot {
		position: absolute;
		left: 0;
		top: 10px;

		width: 36px;
		height: 36px;

		border-radius: 9999px;

		border: 1px solid rgba(197,160,89,0.35);

		background:
			radial-gradient(circle,
			rgba(197,160,89,0.25),
			rgba(197,160,89,0.05));

		box-shadow:
			0 0 40px rgba(197,160,89,0.2);
	}

	.step-label {
		margin-bottom: 12px;

		font-size: 10px;
		letter-spacing: 0.3em;
		text-transform: uppercase;

		color: #c5a059;
	}

	.step-title {
		margin-bottom: 14px;

		font-family: serif;
		font-size: 34px;
		line-height: 1.1;

		color: white;
	}

	.step-desc {
		max-width: 520px;

		font-size: 16px;
		line-height: 1.9;

		color: rgba(255,255,255,0.55);
	}

	.quote-glow {
		position: absolute;
		inset: 0;

		background:
			radial-gradient(circle at top,
			rgba(197,160,89,0.12),
			transparent 65%);
	}

	@media (max-width: 1024px) {
		.image-frame {
			height: 520px;
		}

		.floating-1,
		.floating-2 {
			position: relative;
			inset: unset;

			margin-top: 20px;
		}

		.timeline-step {
			padding-left: 58px;
		}

		.step-title {
			font-size: 28px;
		}
	}

	@media (max-width: 640px) {
		.image-frame {
			height: 420px;
			border-radius: 28px;
		}

		.floating-card {
			padding: 18px 18px;
		}

		.floating-card h3 {
			font-size: 22px;
		}

		.step-title {
			font-size: 24px;
		}

		.step-desc {
			font-size: 15px;
		}
	}
</style>