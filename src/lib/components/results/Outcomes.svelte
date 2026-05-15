<script lang="ts">
	import { onMount } from 'svelte';
    import Outcomes from "$lib/assets/results/2.webp";

	let sectionRef: HTMLElement;
	let timelineRefs: HTMLElement[] = [];
	let imageRef: HTMLElement;
	let quoteRef: HTMLElement;

	const caseStudies = [
		{
			id: '01',
			title: 'Kept a 2.75% Mortgage Instead of Refinancing',
			client: 'Orange County Homeowner',
			before:
				'Client needed liquidity for business expansion but refused to lose a low fixed mortgage rate.',
			strategy:
				'Structured a First Lien HELOC strategy to unlock equity while preserving the existing mortgage.',
			result:
				'Accessed $380K in usable capital while maintaining low monthly obligations and improving liquidity.'
		},
		{
			id: '02',
			title: 'Projected Mortgage Payoff Reduced by 22 Years',
			client: 'Dual-Income Household',
			before:
				'Client was trapped in a traditional 30-year amortization cycle despite strong monthly cash flow.',
			strategy:
				'Implemented income routing and daily interest reduction through a velocity banking structure.',
			result:
				'Projected payoff timeline reduced from 29 years remaining to approximately 7 years.'
		},
		{
			id: '03',
			title: 'Retirement Cash Flow Without Selling Investments',
			client: 'Retired Executive',
			before:
				'Needed additional monthly income but did not want to liquidate appreciated assets.',
			strategy:
				'Combined a proprietary reverse structure with equity access planning.',
			result:
				'Created tax-efficient cash flow while preserving long-term portfolio positioning.'
		}
	];

	onMount(() => {
		let cleanup: (() => void) | undefined;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			const ctx = gsap.context(() => {
				// Timeline reveal
				gsap.from(timelineRefs.filter(Boolean), {
					y: 90,
					opacity: 0,
					stagger: 0.18,
					duration: 1.2,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 78%'
					}
				});

				// Image reveal
				gsap.from(imageRef, {
					scale: 1.15,
					opacity: 0,
					duration: 1.6,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: imageRef,
						start: 'top 82%'
					}
				});

				// Quote
				gsap.from(quoteRef, {
					y: 60,
					opacity: 0,
					duration: 1,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: quoteRef,
						start: 'top 88%'
					}
				});

				// Floating motion
				timelineRefs.forEach((el, i) => {
					gsap.to(el, {
						y: i % 2 === 0 ? -8 : -16,
						duration: 5 + i,
						repeat: -1,
						yoyo: true,
						ease: 'sine.inOut'
					});
				});

				// Parallax image
				gsap.to('.case-image', {
					yPercent: -12,
					scrollTrigger: {
						trigger: imageRef,
						start: 'top bottom',
						end: 'bottom top',
						scrub: true
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
	class="relative overflow-hidden bg-[#050505] py-32 lg:py-44"
>
	<!-- BG -->
	<div class="noise"></div>

	<div class="absolute inset-0 bg-[radial-gradient(circle_at_top,rgba(197,160,89,0.08),transparent_35%)]"></div>

	<div class="vignette"></div>

	<!-- LINE -->
	<div
		class="absolute inset-x-0 top-0 h-px bg-gradient-to-r from-transparent via-white/10 to-transparent"
	></div>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<!-- HEADER -->
		<div class="mb-24 grid gap-16 lg:grid-cols-[0.8fr_1.2fr] lg:items-end">
			<div>
				<div class="section-tag">
					<div class="dot"></div>

					<span>
						Structured Outcomes
					</span>
				</div>

				<h2
					class="mt-8 max-w-xl font-serif text-[clamp(3rem,5vw,5.8rem)] leading-[0.95] tracking-[-0.05em] text-white"
				>
					Real Scenarios.
					<br />
					Real Financial Change.
				</h2>
			</div>

			<div>
				<p class="max-w-2xl text-lg leading-relaxed text-white/58 lg:text-xl">
					The goal isn’t just getting approved.
					It’s restructuring financial outcomes in ways most homeowners and investors
					never realize are possible.
				</p>
			</div>
		</div>

		<!-- MAIN -->
		<div class="grid gap-20 lg:grid-cols-[1.05fr_0.95fr]">
			<!-- LEFT -->
			<div class="relative">
				<div class="timeline-line"></div>

				<div class="space-y-20">
					{#each caseStudies as item, i}
						<div
							bind:this={timelineRefs[i]}
							class="timeline-item"
						>
							<div class="timeline-dot"></div>

							<div class="timeline-id">
								{item.id}
							</div>

							<div class="timeline-content">
								<p class="client">
									{item.client}
								</p>

								<h3>
									{item.title}
								</h3>

								<div class="timeline-copy">
									<div>
										<span>Before</span>

										<p>{item.before}</p>
									</div>

									<div>
										<span>Strategy</span>

										<p>{item.strategy}</p>
									</div>

									<div>
										<span>Outcome</span>

										<p>{item.result}</p>
									</div>
								</div>
							</div>
						</div>
					{/each}
				</div>
			</div>

			<!-- RIGHT -->
			<div class="space-y-8">
				<div
					bind:this={imageRef}
					class="image-wrap"
				>
					<img
						class="case-image"
						src={Outcomes}
						alt="Luxury financial planning"
					/>

					<div class="image-overlay"></div>

					<div class="metric-card">
						<p>
							Average Interest Savings
						</p>

						<h3>$186K+</h3>

						<span>
							Through optimized cash-flow structuring
						</span>
					</div>
				</div>

				<div
					bind:this={quoteRef}
					class="quote-box"
				>
					<div class="quote-mark">“</div>

					<p>
						Most clients don’t need a new mortgage.
						They need a completely different financial structure.
					</p>

					<div class="quote-footer">
						<div class="line"></div>

						<span>
							Wealth With Marcus
						</span>
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

	.vignette {
		position: absolute;
		inset: 0;
		background:
			radial-gradient(circle at center,
			transparent 20%,
			rgba(0,0,0,0.82) 100%);
		pointer-events: none;
	}

	.section-tag {
		display: inline-flex;
		align-items: center;
		gap: 12px;

		padding: 12px 18px;

		border-radius: 9999px;

		border: 1px solid rgba(255,255,255,0.08);

		background: rgba(255,255,255,0.03);

		backdrop-filter: blur(20px);
	}

	.section-tag span {
		font-size: 11px;
		letter-spacing: 0.28em;
		text-transform: uppercase;
		color: rgba(255,255,255,0.55);
		font-family: monospace;
	}

	.dot {
		width: 8px;
		height: 8px;
		border-radius: 9999px;
		background: #c5a059;
		box-shadow: 0 0 18px rgba(197,160,89,0.7);
	}

	.timeline-line {
		position: absolute;
		left: 16px;
		top: 0;
		bottom: 0;
		width: 1px;

		background:
			linear-gradient(
				to bottom,
				rgba(197,160,89,0),
				rgba(197,160,89,0.45),
				rgba(197,160,89,0)
			);
	}

	.timeline-item {
		position: relative;
		padding-left: 72px;
	}

	.timeline-dot {
		position: absolute;
		left: 8px;
		top: 12px;

		width: 16px;
		height: 16px;

		border-radius: 9999px;

		background: #c5a059;

		box-shadow:
			0 0 0 6px rgba(197,160,89,0.08),
			0 0 22px rgba(197,160,89,0.6);
	}

	.timeline-id {
		margin-bottom: 18px;

		font-size: 11px;
		letter-spacing: 0.3em;
		text-transform: uppercase;

		color: #c5a059;
		font-family: monospace;
	}

	.client {
		margin-bottom: 16px;

		font-size: 13px;
		letter-spacing: 0.18em;
		text-transform: uppercase;

		color: rgba(255,255,255,0.38);
	}

	.timeline-content h3 {
		max-width: 680px;

		font-size: clamp(2rem,3vw,3rem);
		line-height: 1.08;
		letter-spacing: -0.04em;

		color: white;
		font-family: serif;
	}

	.timeline-copy {
		margin-top: 34px;

		display: grid;
		gap: 26px;
	}

	.timeline-copy div {
		max-width: 620px;
	}

	.timeline-copy span {
		display: inline-block;
		margin-bottom: 10px;

		font-size: 10px;
		letter-spacing: 0.28em;
		text-transform: uppercase;

		color: #c5a059;
		font-family: monospace;
	}

	.timeline-copy p {
		font-size: 16px;
		line-height: 1.9;
		color: rgba(255,255,255,0.58);
	}

	.image-wrap {
		position: relative;
		overflow: hidden;

		border-radius: 34px;

		border: 1px solid rgba(255,255,255,0.08);

		height: 760px;
	}

	.case-image {
		width: 100%;
		height: 120%;
		object-fit: cover;
	}

	.image-overlay {
		position: absolute;
		inset: 0;

		background:
			linear-gradient(to top,
			rgba(0,0,0,0.92),
			transparent 40%);
	}

	.metric-card {
		position: absolute;
		left: 28px;
		right: 28px;
		bottom: 28px;

		padding: 28px;

		border-radius: 28px;

		border: 1px solid rgba(255,255,255,0.08);

		background: rgba(10,10,10,0.7);

		backdrop-filter: blur(24px);
	}

	.metric-card p {
		font-size: 11px;
		letter-spacing: 0.28em;
		text-transform: uppercase;

		color: rgba(255,255,255,0.45);

		font-family: monospace;
	}

	.metric-card h3 {
		margin-top: 18px;

		font-size: clamp(3rem,5vw,5rem);
		line-height: 1;

		color: white;
		font-family: serif;
	}

	.metric-card span {
		display: block;
		margin-top: 16px;

		font-size: 15px;
		line-height: 1.8;

		color: rgba(255,255,255,0.52);
	}

	.quote-box {
		position: relative;
		overflow: hidden;

		padding: 42px;

		border-radius: 32px;

		border: 1px solid rgba(255,255,255,0.08);

		background: rgba(255,255,255,0.03);

		backdrop-filter: blur(20px);
	}

	.quote-mark {
		position: absolute;
		right: 28px;
		top: 10px;

		font-size: 140px;
		line-height: 1;

		color: rgba(197,160,89,0.12);

		font-family: serif;
	}

	.quote-box p {
		position: relative;
		z-index: 2;

		max-width: 580px;

		font-size: clamp(1.8rem,3vw,2.8rem);
		line-height: 1.2;
		letter-spacing: -0.04em;

		color: white;

		font-family: serif;
	}

	.quote-footer {
		display: flex;
		align-items: center;
		gap: 14px;

		margin-top: 34px;
	}

	.quote-footer .line {
		width: 70px;
		height: 1px;

		background: rgba(197,160,89,0.5);
	}

	.quote-footer span {
		font-size: 11px;
		letter-spacing: 0.26em;
		text-transform: uppercase;

		color: rgba(255,255,255,0.45);

		font-family: monospace;
	}

	@media (max-width: 1024px) {
		.image-wrap {
			height: 560px;
		}
	}

	@media (max-width: 640px) {
		.timeline-item {
			padding-left: 54px;
		}

		.timeline-content h3 {
			font-size: 2rem;
		}

		.quote-box {
			padding: 32px;
		}

		.quote-box p {
			font-size: 1.8rem;
		}

		.image-wrap {
			height: 440px;
		}
	}
</style>    