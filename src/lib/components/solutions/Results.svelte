<script lang="ts">
	import { onMount } from 'svelte';

	let sectionRef: HTMLElement;
	let cards: HTMLElement[] = $state([]);
	let titleRef: HTMLElement;

	const caseStudies = [
		{
			tag: 'FIRST LIEN HELOC',
			title: '30-Year Mortgage → 7-Year Payoff Strategy',
			before: [
				'$612,000 Mortgage Balance',
				'30-Year Fixed Loan',
				'$4,980 Monthly Obligation',
				'Little Principal Reduction'
			],
			after: [
				'Projected 7-Year Payoff',
				'Interest Reduced Significantly',
				'Income Redirected Strategically',
				'Cash Flow Control Increased'
			],
			stats: {
				label: 'Projected Interest Reduction',
				value: '$480K+'
			}
		},
		{
			tag: 'DIGITAL HELOC',
			title: 'Unlocked Equity Without Refinancing',
			before: [
				'2.9% Existing Mortgage',
				'Needed $180K Liquidity',
				'Traditional Refinance Destroyed Rate',
				'Limited Flexibility'
			],
			after: [
				'Maintained Existing Mortgage',
				'Accessed Equity Fast',
				'Digital Approval Experience',
				'Preserved Cash Flow'
			],
			stats: {
				label: 'Equity Accessed',
				value: '$180K'
			}
		},
		{
			tag: 'PROPRIETARY REVERSE',
			title: 'Retirement Cash Flow Optimization',
			before: [
				'$2.1M Home Equity',
				'Monthly Mortgage Stress',
				'Limited Retirement Liquidity',
				'Assets Locked in Property'
			],
			after: [
				'No Mandatory Mortgage Payments',
				'Liquidity Without Selling',
				'Improved Retirement Cash Flow',
				'Protected Long-Term Assets'
			],
			stats: {
				label: 'Available Equity Strategy',
				value: '$1.2M'
			}
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
					duration: 1.1,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: titleRef,
						start: 'top 82%'
					}
				});

				gsap.from(cards, {
					y: 120,
					opacity: 0,
					duration: 1.2,
					stagger: 0.15,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: cards[0],
						start: 'top 88%'
					}
				});

				cards.forEach((card) => {
					card.addEventListener('mousemove', (e: MouseEvent) => {
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

					card.addEventListener('mouseleave', () => {
						gsap.to(card, {
							rotateX: 0,
							rotateY: 0,
							duration: 1,
							ease: 'elastic.out(1,0.5)'
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
	<!-- BACKGROUND -->
	<div class="noise"></div>

	<div class="mesh mesh-1"></div>
	<div class="mesh mesh-2"></div>

	<div class="vignette"></div>

	<!-- TOP LINE -->
	<div
		class="absolute inset-x-0 top-0 h-px bg-gradient-to-r from-transparent via-white/10 to-transparent"
	></div>

	<!-- SVG FLOW -->
	<svg
		class="pointer-events-none absolute inset-0 h-full w-full opacity-[0.06]"
		preserveAspectRatio="none"
	>
		<defs>
			<linearGradient id="case-line" x1="0%" y1="0%" x2="100%" y2="0%">
				<stop offset="0%" stop-color="#c5a059" stop-opacity="0" />
				<stop offset="50%" stop-color="#c5a059" stop-opacity="1" />
				<stop offset="100%" stop-color="#c5a059" stop-opacity="0" />
			</linearGradient>
		</defs>

		<path
			d="M0,120 C400,220 800,20 1200,120 S1700,260 1920,160"
			stroke="url(#case-line)"
			stroke-width="1"
			fill="none"
		>
			<animate
				attributeName="d"
				dur="18s"
				repeatCount="indefinite"
				values="
        M0,120 C400,220 800,20 1200,120 S1700,260 1920,160;
        M0,180 C500,80 900,250 1200,80 S1600,120 1920,220;
        M0,120 C400,220 800,20 1200,120 S1700,260 1920,160"
			/>
		</path>
	</svg>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<!-- HEADER -->
		<div
			bind:this={titleRef}
			class="mb-20 flex flex-col gap-10 lg:flex-row lg:items-end lg:justify-between"
		>
			<div class="max-w-3xl">
				<div class="eyebrow">
					<div class="dot"></div>

					<span>Results / Financial Transformation</span>
				</div>

				<h2
					class="mt-8 font-serif text-[clamp(3rem,6vw,6rem)] leading-[0.95] tracking-[-0.04em] text-white"
				>
					Real Strategy.
					<br />
					Real Financial Outcomes.
				</h2>
			</div>

			<p class="max-w-xl text-lg leading-relaxed text-white/50">
				This is not about “getting a loan.”
				It’s about restructuring how money flows through your life — reducing
				friction, increasing control, and accelerating long-term wealth positioning.
			</p>
		</div>

		<!-- CASE STUDIES -->
		<div class="grid grid-cols-1 gap-8 xl:grid-cols-3">
			{#each caseStudies as study, i}
				<div
					bind:this={cards[i]}
					class="case-card"
				>
					<!-- glow -->
					<div class="card-glow"></div>

					<!-- shine -->
					<div class="shine"></div>

					<!-- content -->
					<div class="relative z-10">
						<div class="mb-8 flex items-center justify-between gap-4">
							<div class="pill">
								{study.tag}
							</div>

							<div class="mini-stat">
								<span>{study.stats.label}</span>

								<strong>{study.stats.value}</strong>
							</div>
						</div>

						<h3
							class="max-w-sm font-serif text-3xl leading-[1.05] tracking-[-0.03em] text-white"
						>
							{study.title}
						</h3>

						<div class="mt-12 grid gap-6">
							<!-- BEFORE -->
							<div class="compare-block before">
								<div class="compare-label">
									Before
								</div>

								<div class="space-y-4">
									{#each study.before as item}
										<div class="compare-row">
											<div class="bullet red"></div>

											<p>{item}</p>
										</div>
									{/each}
								</div>
							</div>

							<!-- AFTER -->
							<div class="compare-block after">
								<div class="compare-label">
									After
								</div>

								<div class="space-y-4">
									{#each study.after as item}
										<div class="compare-row">
											<div class="bullet gold"></div>

											<p>{item}</p>
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
			radial-gradient(rgba(255,255,255,0.3) 0.5px, transparent 0.5px);

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

	.mesh {
		position: absolute;
		border-radius: 9999px;
		filter: blur(140px);
	}

	.mesh-1 {
		top: -200px;
		left: -200px;

		width: 700px;
		height: 700px;

		background: rgba(197,160,89,0.1);
	}

	.mesh-2 {
		right: -240px;
		bottom: -240px;

		width: 700px;
		height: 700px;

		background: rgba(197,160,89,0.08);
	}

	.eyebrow {
		display: inline-flex;
		align-items: center;
		gap: 14px;

		padding: 12px 18px;

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

	.case-card {
		position: relative;
		overflow: hidden;

		padding: 34px;

		border-radius: 34px;

		border: 1px solid rgba(255,255,255,0.08);

		background: rgba(255,255,255,0.03);

		backdrop-filter: blur(28px);

		transition:
			transform 0.5s ease,
			border-color 0.5s ease,
			box-shadow 0.5s ease;

		transform-style: preserve-3d;
	}

	.case-card:hover {
		border-color: rgba(197,160,89,0.35);

		box-shadow:
			0 0 80px rgba(197,160,89,0.08);
	}

	.card-glow {
		position: absolute;
		inset: 0;

		opacity: 0;

		background:
			radial-gradient(circle at top,
			rgba(197,160,89,0.16),
			transparent 70%);

		transition: opacity 0.6s ease;
	}

	.case-card:hover .card-glow {
		opacity: 1;
	}

	.shine {
		position: absolute;
		inset: 0;

		background:
			linear-gradient(
				115deg,
				transparent 40%,
				rgba(255,255,255,0.05) 50%,
				transparent 60%
			);

		transform: translateX(-120%) rotate(12deg);

		animation: shineMove 8s linear infinite;
	}

	.pill {
		padding: 10px 14px;

		border-radius: 9999px;

		border: 1px solid rgba(197,160,89,0.2);

		background: rgba(197,160,89,0.08);

		font-size: 10px;
		letter-spacing: 0.24em;
		text-transform: uppercase;
		font-family: monospace;

		color: #c5a059;
	}

	.mini-stat {
		text-align: right;
	}

	.mini-stat span {
		display: block;

		font-size: 10px;
		letter-spacing: 0.12em;
		text-transform: uppercase;

		color: rgba(255,255,255,0.35);
	}

	.mini-stat strong {
		display: block;
		margin-top: 4px;

		font-size: 22px;
		font-family: serif;

		color: white;
	}

	.compare-block {
		padding: 24px;

		border-radius: 24px;
		border: 1px solid rgba(255,255,255,0.06);
	}

	.compare-block.before {
		background: rgba(255,255,255,0.02);
	}

	.compare-block.after {
		background:
			linear-gradient(
				180deg,
				rgba(197,160,89,0.08),
				rgba(255,255,255,0.02)
			);
	}

	.compare-label {
		margin-bottom: 20px;

		font-size: 10px;
		letter-spacing: 0.28em;
		text-transform: uppercase;
		font-family: monospace;

		color: rgba(255,255,255,0.35);
	}

	.compare-row {
		display: flex;
		align-items: center;
		gap: 14px;
	}

	.compare-row p {
		font-size: 14px;
		line-height: 1.6;

		color: rgba(255,255,255,0.7);
	}

	.bullet {
		width: 7px;
		height: 7px;
		border-radius: 9999px;

		flex-shrink: 0;
	}

	.bullet.red {
		background: #ff5f5f;
		box-shadow: 0 0 16px rgba(255,95,95,0.5);
	}

	.bullet.gold {
		background: #c5a059;
		box-shadow: 0 0 18px rgba(197,160,89,0.7);
	}

	.case-btn {
		width: 100%;

		display: flex;
		align-items: center;
		justify-content: center;
		gap: 12px;

		padding: 18px 22px;

		border-radius: 9999px;

		background: white;

		color: black;

		font-size: 14px;
		font-weight: 600;

		transition:
			transform 0.4s ease,
			box-shadow 0.4s ease;
	}

	.case-btn:hover {
		transform: translateY(-2px);

		box-shadow:
			0 0 50px rgba(255,255,255,0.12);
	}

	@keyframes shineMove {
		from {
			transform: translateX(-120%) rotate(12deg);
		}

		to {
			transform: translateX(120%) rotate(12deg);
		}
	}
</style>