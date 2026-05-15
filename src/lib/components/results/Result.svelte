<script lang="ts">
	import { onMount } from 'svelte';

	let sectionRef: HTMLElement;
	let titleRef: HTMLElement;
	let statRefs: HTMLElement[] = [];
	let lineRef: HTMLElement;

	const results = [
		{
			number: '$487K',
			label: 'Equity Accessed',
			desc: 'Without refinancing a 2.9% first mortgage.'
		},
		{
			number: '22 Years',
			label: 'Mortgage Time Reduced',
			desc: 'Using First Lien HELOC cash flow optimization.'
		},
		{
			number: '$4,120',
			label: 'Monthly Cash Flow Freed',
			desc: 'After restructuring revolving debt and mortgage interest.'
		},
		{
			number: '0',
			label: 'Monthly Mortgage Payments',
			desc: 'Through reverse mortgage restructuring.'
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
					y: 90,
					opacity: 0,
					duration: 1.2,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: titleRef,
						start: 'top 82%'
					}
				});

				gsap.from(lineRef, {
					scaleX: 0,
					transformOrigin: 'left',
					duration: 1.6,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: lineRef,
						start: 'top 90%'
					}
				});

				gsap.from(statRefs, {
					y: 80,
					opacity: 0,
					stagger: 0.12,
					duration: 1.1,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 72%'
					}
				});

				statRefs.forEach((card) => {
					card.addEventListener('mousemove', (e: MouseEvent) => {
						const rect = card.getBoundingClientRect();

						const x = e.clientX - rect.left;
						const y = e.clientY - rect.top;

						gsap.to(card, {
							rotateY: gsap.utils.mapRange(0, rect.width, -7, 7, x),
							rotateX: gsap.utils.mapRange(0, rect.height, 7, -7, y),
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
							ease: 'elastic.out(1,0.45)'
						});
					});
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
	<!-- BACKGROUND -->
	<div class="noise"></div>

	<div class="mesh mesh-1"></div>
	<div class="mesh mesh-2"></div>

	<div class="vignette"></div>

	<!-- GRID -->
	<div class="grid-overlay"></div>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<!-- HEADER -->
		<div class="mb-20 max-w-4xl">
			<div
				class="mb-7 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl"
			>
				<div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

				<span class="font-mono text-[11px] uppercase tracking-[0.35em] text-white/50">
					Results That Change Financial Trajectories
				</span>
			</div>

			<h2
				bind:this={titleRef}
				class="max-w-5xl font-serif text-[clamp(3rem,6vw,6.5rem)] leading-[0.92] tracking-[-0.05em] text-white"
			>
				Most Clients Don’t Need
				<span class="italic text-[#c5a059]"> Another Loan.</span>

				<br />

				They Need A Better
				<span class="italic text-[#c5a059]"> Financial Structure.</span>
			</h2>

			<p class="mt-10 max-w-3xl text-xl leading-relaxed text-white/55">
				Every strategy below was designed to improve control, accelerate equity,
				and create flexibility without destroying existing financial positions.
			</p>
		</div>

		<!-- LINE -->
		<div
			bind:this={lineRef}
			class="mb-20 h-px w-full bg-gradient-to-r from-transparent via-[#c5a059]/40 to-transparent"
		></div>

		<!-- STATS -->
		<div class="grid grid-cols-1 gap-6 md:grid-cols-2 xl:grid-cols-4">
			{#each results as item, i}
				<div
					bind:this={statRefs[i]}
					class="result-card"
				>
					<div class="card-glow"></div>

					<div class="relative z-10">
						<div class="mb-8">
							<p
								class="font-serif text-[clamp(2.6rem,5vw,4.6rem)] leading-none tracking-[-0.05em] text-white"
							>
								{item.number}
							</p>
						</div>

						<div class="mb-5 h-px w-full bg-gradient-to-r from-[#c5a059]/40 to-transparent"></div>

						<h3
							class="mb-4 text-lg font-medium tracking-[-0.02em] text-white"
						>
							{item.label}
						</h3>

						<p class="text-sm leading-relaxed text-white/50">
							{item.desc}
						</p>
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
			radial-gradient(rgba(255,255,255,0.35) 0.5px, transparent 0.5px);

		background-size: 4px 4px;
	}

	.grid-overlay {
		position: absolute;
		inset: 0;

		background-image:
			linear-gradient(rgba(255,255,255,0.03) 1px, transparent 1px),
			linear-gradient(90deg, rgba(255,255,255,0.03) 1px, transparent 1px);

		background-size: 120px 120px;

		mask-image: radial-gradient(circle at center, black, transparent 90%);
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
		top: -300px;
		left: -200px;

		width: 800px;
		height: 800px;

		background: rgba(197,160,89,0.1);
	}

	.mesh-2 {
		right: -300px;
		bottom: -300px;

		width: 900px;
		height: 900px;

		background: rgba(197,160,89,0.08);
	}

	.result-card {
		position: relative;
		overflow: hidden;

		min-height: 320px;

		padding: 34px;

		border-radius: 34px;

		border: 1px solid rgba(255,255,255,0.08);

		background:
			linear-gradient(
				180deg,
				rgba(255,255,255,0.05),
				rgba(255,255,255,0.025)
			);

		backdrop-filter: blur(24px);

		transform-style: preserve-3d;

		transition:
			border-color 0.5s ease,
			box-shadow 0.5s ease,
			transform 0.5s ease;
	}

	.result-card:hover {
		border-color: rgba(197,160,89,0.35);

		box-shadow:
			0 0 80px rgba(197,160,89,0.08);
	}

	.card-glow {
		position: absolute;
		inset: 0;

		background:
			radial-gradient(circle at top,
			rgba(197,160,89,0.16),
			transparent 70%);

		opacity: 0;

		transition: opacity 0.5s ease;
	}

	.result-card:hover .card-glow {
		opacity: 1;
	}
</style>