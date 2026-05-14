<script lang="ts">
	import { onMount } from 'svelte';

	let sectionRef: HTMLElement;
	let cards: HTMLElement[] = $state([]);
	let glowRef: HTMLElement;

	const solutions = [
		{
			kicker: 'Home Equity',
			title: '5-Day Digital HELOC',
			desc:
				'Access liquidity without touching your low-rate mortgage. Fast approvals. Digital process. Strategic equity deployment.',
			stats: ['Funding in Days', 'No Traditional Refi', 'Cash Flow Control'],
			tag: 'Traffic Driver',
			accent: 'from-[#c5a059]/30 via-[#c5a059]/5 to-transparent'
		},
		{
			kicker: 'Wealth Strategy',
			title: 'First Lien HELOC',
			desc:
				'Transform income flow into a debt acceleration system designed to compress payoff timelines and maximize liquidity.',
			stats: ['Payoff Velocity', 'Interest Optimization', 'Equity Acceleration'],
			tag: 'Core Revenue',
			accent: 'from-white/20 via-white/[0.03] to-transparent'
		},
		{
			kicker: 'Retirement Capital',
			title: 'Reverse Mortgage',
			desc:
				'Protect cash flow, eliminate mandatory mortgage payments, and unlock tax-efficient equity strategies.',
			stats: ['No Monthly Payment', 'Retain Ownership', 'Ages 62+'],
			tag: 'High Intent',
			accent: 'from-[#c5a059]/20 via-transparent to-transparent'
		}
	];

	onMount(() => {
	let cleanup: (() => void) | undefined;

	const init = async () => {
		const gsap = (await import('gsap')).default;
		const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

		gsap.registerPlugin(ScrollTrigger);

		const ctx = gsap.context(() => {
            gsap.from(cards, {
				y: 120,
				opacity: 0,
				duration: 1.2,
				stagger: 0.14,
				ease: 'power4.out',
				scrollTrigger: {
					trigger: sectionRef,
					start: 'top 72%'
				}
			});

			cards.forEach((card) => {
				card.addEventListener('mousemove', (e: MouseEvent) => {
					const rect = card.getBoundingClientRect();

					const x = e.clientX - rect.left;
					const y = e.clientY - rect.top;

					const rotateY = gsap.utils.mapRange(0, rect.width, -10, 10, x);
					const rotateX = gsap.utils.mapRange(0, rect.height, 10, -10, y);

					gsap.to(card, {
						rotateX,
						rotateY,
						transformPerspective: 1400,
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

			const xTo = gsap.quickTo(glowRef, 'x', {
				duration: 0.7,
				ease: 'power3.out'
			});

			const yTo = gsap.quickTo(glowRef, 'y', {
				duration: 0.7,
				ease: 'power3.out'
			});

			const moveGlow = (e: MouseEvent) => {
				const rect = sectionRef.getBoundingClientRect();

				xTo(e.clientX - rect.left - 250);
				yTo(e.clientY - rect.top - 250);
			};

			sectionRef.addEventListener('mousemove', moveGlow);

			return () => {
				sectionRef?.removeEventListener('mousemove', moveGlow);
			};
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
	class="relative overflow-hidden bg-[#050505] py-32 lg:py-44"
>
	<!-- BACKGROUND -->
	<div class="noise"></div>

	<div class="absolute inset-0 opacity-[0.05]">
		<div class="grid-bg"></div>
	</div>

	<div class="mesh mesh-1"></div>
	<div class="mesh mesh-2"></div>

	<div class="vignette"></div>

	<!-- SPOTLIGHT -->
	<div
		bind:this={glowRef}
		class="pointer-events-none absolute left-0 top-0 h-[500px] w-[500px] rounded-full bg-[#c5a059]/10 blur-[120px]"
	></div>

	<!-- GOLD THREAD -->
	<svg
		class="pointer-events-none absolute inset-0 h-full w-full opacity-[0.08]"
		preserveAspectRatio="none"
	>
		<defs>
			<linearGradient id="solution-gold" x1="0%" y1="0%" x2="100%" y2="0%">
				<stop offset="0%" stop-color="#c5a059" stop-opacity="0" />
				<stop offset="50%" stop-color="#c5a059" stop-opacity="1" />
				<stop offset="100%" stop-color="#c5a059" stop-opacity="0" />
			</linearGradient>
		</defs>

		<path
			d="M0,200 C300,80 700,300 1100,180 S1600,50 1920,220"
			stroke="url(#solution-gold)"
			stroke-width="1"
			fill="none"
		>
			<animate
				attributeName="d"
				dur="18s"
				repeatCount="indefinite"
				values="
				M0,200 C300,80 700,300 1100,180 S1600,50 1920,220;
				M0,240 C420,100 760,260 1100,140 S1580,120 1920,180;
				M0,200 C300,80 700,300 1100,180 S1600,50 1920,220"
			/>
		</path>
	</svg>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<!-- HEADER -->
		<div class="mb-20 max-w-4xl">
			<div
				class="mb-8 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl"
			>
				<div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

				<span class="font-mono text-[11px] uppercase tracking-[0.35em] text-white/50">
					Structured Capital Solutions
				</span>
			</div>

			<h2
				class="font-serif text-[clamp(3.2rem,7vw,7rem)] leading-[0.92] tracking-[-0.05em] text-white"
			>
				Most homeowners<br />
				don’t need a new loan.<br />

				<span class="text-[#c5a059]">
					They need a better strategy.
				</span>
			</h2>

			<p
				class="mt-10 max-w-2xl text-lg leading-relaxed text-white/55 lg:text-xl"
			>
				Your mortgage should be a financial tool — not a financial trap.
				We build lending structures designed to increase liquidity, preserve
				low-rate positions, and optimize long-term cash flow.
			</p>
		</div>

		<!-- CARDS -->
		<div class="grid grid-cols-1 gap-8 lg:grid-cols-3">
			{#each solutions as item, i}
				<div
					bind:this={cards[i]}
					class="solution-card group"
				>
					<!-- GRADIENT -->
					<div class={`absolute inset-0 bg-gradient-to-br ${item.accent}`}></div>

					<!-- SHINE -->
					<div class="shine"></div>

					<!-- BORDER GLOW -->
					<div class="border-glow"></div>

					<div class="relative z-10 flex h-fit flex-col">
						<!-- TOP -->
						<div class="mb-12 flex items-start justify-between gap-4">
							<div>
								<p
									class="mb-4 font-mono text-[11px] uppercase tracking-[0.28em] text-white/35"
								>
									{item.kicker}
								</p>

								<h3
									class="max-w-[240px] font-serif text-4xl leading-[1] tracking-[-0.03em] text-white"
								>
									{item.title}
								</h3>
							</div>

						</div>

						<p
							class="mb-10 text-[15px] leading-[1.9] text-white/55"
						>
							{item.desc}
						</p>

						<!-- STATS -->
						<div class="mb-12 space-y-4">
							{#each item.stats as stat}
								<div
									class="flex items-center gap-4 border-b border-white/6 pb-4"
								>
									<div
										class="h-1.5 w-1.5 rounded-full bg-[#c5a059]"
									></div>

									<p
										class="font-mono text-[11px] uppercase tracking-[0.24em] text-white/60"
									>
										{stat}
									</p>
								</div>
							{/each}
						</div>

						<!-- CTA -->
						<div class="mt-10">
							<button class="solution-btn">
								<span class="relative z-10 flex items-center gap-3">
									Explore Strategy

									<svg
										class="h-4 w-4 transition-transform duration-500 group-hover:translate-x-1"
										fill="none"
										viewBox="0 0 24 24"
										stroke="currentColor"
										stroke-width="2.2"
									>
										<path
											stroke-linecap="round"
											stroke-linejoin="round"
											d="M17 8l4 4m0 0l-4 4m4-4H3"
										/>
									</svg>
								</span>

								<div class="btn-shine"></div>
							</button>
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
			radial-gradient(rgba(255,255,255,0.4) 0.5px, transparent 0.5px);

		background-size: 4px 4px;
	}

	.grid-bg {
		position: absolute;
		inset: 0;

		background-image:
			linear-gradient(rgba(255,255,255,0.03) 1px, transparent 1px),
			linear-gradient(90deg, rgba(255,255,255,0.03) 1px, transparent 1px);

		background-size: 90px 90px;

		mask-image: radial-gradient(circle at center, black, transparent 85%);
	}

	.mesh {
		position: absolute;
		border-radius: 9999px;
		filter: blur(120px);
	}

	.mesh-1 {
		top: -250px;
		left: -200px;
		width: 700px;
		height: 700px;

		background: rgba(197,160,89,0.1);
	}

	.mesh-2 {
		right: -300px;
		bottom: -300px;
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
	}

	.solution-card {
		position: relative;
		overflow: hidden;

		min-height: 620px;

		padding: 36px;

		border-radius: 36px;

		border: 1px solid rgba(255,255,255,0.08);

		background: rgba(255,255,255,0.035);

		backdrop-filter: blur(24px);

		transform-style: preserve-3d;

		transition:
			transform 0.5s ease,
			border-color 0.5s ease,
			box-shadow 0.5s ease;
	}

	.solution-card:hover {
		border-color: rgba(197,160,89,0.28);

		box-shadow:
			0 0 80px rgba(197,160,89,0.08);
	}

	.border-glow {
		position: absolute;
		inset: 0;

		border-radius: inherit;

		padding: 1px;

		background:
			linear-gradient(
				180deg,
				rgba(255,255,255,0.15),
				transparent,
				rgba(197,160,89,0.15)
			);

		mask:
			linear-gradient(#fff 0 0) content-box,
			linear-gradient(#fff 0 0);

		mask-composite: exclude;

		opacity: 0.6;
	}

	.shine {
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

		animation: shineMove 8s linear infinite;
	}

	.solution-btn {
		position: relative;
		overflow: hidden;

		width: 100%;

		border-radius: 9999px;

		border: 1px solid rgba(255,255,255,0.08);

		background: rgba(255,255,255,0.06);

		padding: 18px 24px;

		color: white;

		font-size: 13px;
		font-weight: 600;

		transition:
			transform 0.45s ease,
			background 0.45s ease,
			border-color 0.45s ease;
	}

	.solution-btn:hover {
		background: #c5a059;
		color: black;

		border-color: #c5a059;

		transform: translateY(-2px);
	}

	.btn-shine {
		position: absolute;
		inset: 0;

		background:
			linear-gradient(
				115deg,
				transparent 35%,
				rgba(255,255,255,0.45) 50%,
				transparent 65%
			);

		transform: translateX(-120%);

		transition: transform 0.8s ease;
	}

	.solution-btn:hover .btn-shine {
		transform: translateX(120%);
	}

	@keyframes shineMove {
		from {
			transform: translateX(-120%) rotate(12deg);
		}

		to {
			transform: translateX(120%) rotate(12deg);
		}
	}

	@media (max-width: 768px) {
		.solution-card {
			min-height: auto;
			padding: 28px;
		}
	}
</style>