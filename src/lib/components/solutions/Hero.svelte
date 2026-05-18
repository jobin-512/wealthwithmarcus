<script lang="ts">
	import { onMount } from 'svelte';

	let sectionRef: HTMLElement;

	const solutionGroups = [
		{
			tag: 'HOMEOWNERS',
			title: 'Unlock Equity Without Destroying Your Existing Mortgage',
			copy:
				'Most homeowners are trapped by one thing — they think refinancing is the only option. It isn’t. Access liquidity, optimize cash flow, and keep your low-interest position intact.',
			features: [
				'5-Day Digital HELOC',
				'First Lien HELOC Strategy',
				'Reverse Mortgage Solutions',
				'Equity Access Without Refinance'
			],
			stats: [
				{ value: '$500K+', label: 'Average Equity Accessed' },
				{ value: '5 Days', label: 'Fast Digital Funding' }
			]
		},
		{
			tag: 'HIGH NET WORTH',
			title: 'Sophisticated Lending Strategies Built For Capital Efficiency',
			copy:
				'Your mortgage should work like a financial instrument — not a burden. Structure liquidity, preserve assets, and create leverage without unnecessary tax or cash flow pressure.',
			features: [
				'First Lien HELOC',
				'Portfolio-Based Lending',
				'Equity Velocity Banking',
				'Cash Flow Optimization'
			],
			stats: [
				{ value: '$4M', label: 'Proprietary Reverse Limits' },
				{ value: '24/7', label: 'Strategic Support Access' }
			]
		},
		{
			tag: 'INVESTORS',
			title: 'Deploy Capital Faster And Scale With Less Friction',
			copy:
				'Real estate investors don’t win with cheap rates alone. They win with speed, flexibility, and access to intelligent financing structures.',
			features: [
				'DSCR Loans',
				'Fix & Flip Financing',
				'Bridge Loans',
				'Portfolio Expansion Lending'
			],
			stats: [
				{ value: '90%', label: 'LTV Available' },
				{ value: '48H', label: 'Fast Pre-Qualification' }
			]
		}
	];

	onMount(() => {
		let cleanup: (() => void) | undefined;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			const cards = sectionRef.querySelectorAll('.solution-card');

			const ctx = gsap.context(() => {
				gsap.from(cards, {
					y: 120,
					opacity: 0,
					stagger: 0.18,
					duration: 1.3,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 72%'
					}
				});

				cards.forEach((card) => {
					card.addEventListener('mousemove', (e: MouseEvent) => {
						const rect = (card as HTMLElement).getBoundingClientRect();

						const x = e.clientX - rect.left;
						const y = e.clientY - rect.top;

						gsap.to(card, {
							rotateY: gsap.utils.mapRange(0, rect.width, -8, 8, x),
							rotateX: gsap.utils.mapRange(0, rect.height, 8, -8, y),
							transformPerspective: 1200,
							duration: 0.5,
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
	<!-- Background -->
	<div class="noise"></div>

	<div class="absolute inset-0">
		<div class="orb orb-1"></div>
		<div class="orb orb-2"></div>
	</div>

	<div class="vignette"></div>

	<!-- Header -->
	<div class="relative z-10 mx-auto mb-24 max-w-7xl px-6">
		<div class="max-w-3xl">
			<div
				class="mb-7 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl"
			>
				<div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

				<span class="font-mono text-[11px] uppercase tracking-[0.32em] text-white/55">
					Solutions Built Around Strategy
				</span>
			</div>

			<h2
				class="font-serif text-[clamp(3rem,6vw,6rem)] leading-[0.95] tracking-[-0.04em] text-white"
			>
				One System.
				<br />
				<span class="italic text-[#c5a059]">Multiple Financial Paths.</span>
			</h2>

			<p class="mt-8 max-w-2xl text-lg leading-relaxed text-white/55 lg:text-xl">
				Every client has different leverage, equity, and cash flow goals. Our
				structuring approach adapts around the outcome you actually want.
			</p>
		</div>
	</div>

	<!-- Cards -->
	<div class="relative z-10 mx-auto grid max-w-7xl grid-cols-1 gap-8 px-6 lg:grid-cols-3">
		{#each solutionGroups as item}
			<div class="solution-card">
				<!-- Glow -->
				<div class="card-glow"></div>

				<!-- Shine -->
				<div class="card-shine"></div>

				<div class="relative z-10">
					<!-- Top -->
					<div class="mb-10">
						<p
							class="mb-5 font-mono text-[11px] uppercase tracking-[0.3em] text-[#c5a059]"
						>
							{item.tag}
						</p>

						<h3
							class="mb-6 font-serif text-[2rem] leading-[1.05] tracking-[-0.03em] text-white"
						>
							{item.title}
						</h3>

						<p class="text-[15px] leading-relaxed text-white/55">
							{item.copy}
						</p>
					</div>

					<!-- Features -->
					<div class="space-y-4 border-t border-white/10 pt-8">
						{#each item.features as feature}
							<div class="feature-row">
								<div class="feature-dot"></div>

								<span>{feature}</span>
							</div>
						{/each}
					</div>

					<!-- Stats -->
					<div class="mt-10 grid grid-cols-2 gap-4">
						{#each item.stats as stat}
							<div class="stat-box">
								<h4>{stat.value}</h4>

								<p>{stat.label}</p>
							</div>
						{/each}
					</div>

				</div>
			</div>
		{/each}
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
		background-image: radial-gradient(rgba(255,255,255,0.4) 0.5px, transparent 0.5px);
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

	.orb {
		position: absolute;
		border-radius: 9999px;
		filter: blur(140px);
	}

	.orb-1 {
		top: -250px;
		left: -200px;
		width: 700px;
		height: 700px;
		background: rgba(197,160,89,0.12);
	}

	.orb-2 {
		right: -250px;
		bottom: -300px;
		width: 800px;
		height: 800px;
		background: rgba(197,160,89,0.08);
	}

	.solution-card {
		position: relative;
		overflow: hidden;

		padding: 40px;

		border-radius: 36px;

		border: 1px solid rgba(255,255,255,0.08);

		background: rgba(255,255,255,0.03);

		backdrop-filter: blur(24px);

		transition:
			transform 0.5s ease,
			border-color 0.5s ease,
			box-shadow 0.5s ease;
	}

	.solution-card:hover {
		border-color: rgba(197,160,89,0.35);

		box-shadow:
			0 0 80px rgba(197,160,89,0.08);
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

	.solution-card:hover .card-glow {
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

	.feature-row {
		display: flex;
		align-items: center;
		gap: 14px;

		color: rgba(255,255,255,0.72);

		font-size: 14px;

		transition: all 0.35s ease;
	}

	.feature-row:hover {
		transform: translateX(6px);
		color: white;
	}

	.feature-dot {
		width: 7px;
		height: 7px;
		border-radius: 9999px;
		background: #c5a059;

		box-shadow: 0 0 18px rgba(197,160,89,0.7);
	}

	.stat-box {
		padding: 20px;
		border-radius: 22px;

		border: 1px solid rgba(255,255,255,0.08);

		background: rgba(255,255,255,0.025);
	}

	.stat-box h4 {
		font-size: 1.8rem;
		font-family: serif;
		color: white;
		margin-bottom: 6px;
	}

	.stat-box p {
		font-size: 12px;
		text-transform: uppercase;
		letter-spacing: 0.18em;
		color: rgba(255,255,255,0.4);
	}

	.cta-btn {
		width: 100%;
		margin-top: 30px;

		display: flex;
		align-items: center;
		justify-content: center;
		gap: 12px;

		padding: 18px;

		border-radius: 9999px;

		background: white;

		color: black;

		font-size: 14px;
		font-weight: 600;

		transition: all 0.4s ease;
	}

	.cta-btn:hover {
		transform: scale(1.02);

		box-shadow:
			0 0 50px rgba(255,255,255,0.15);
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