<script lang="ts">
	import { onMount } from 'svelte';
    import Hero from "$lib/assets/results/1.webp";

	let sectionRef: HTMLElement;
	let spotlightRef: HTMLElement;
	let titleWords: HTMLElement[] = [];
	let statsRefs: HTMLElement[] = [];

	const results = [
		{
			number: '$412K',
			label: 'Equity Accessed',
			desc: 'Without refinancing a 2.9% first mortgage.',
			tag: 'Reverse Strategy'
		},
		{
			number: '7.2 YRS',
			label: 'Mortgage Payoff Projection',
			desc: 'Using a First Lien HELOC cash flow strategy.',
			tag: 'Velocity Banking'
		},
		{
			number: '$3.1M',
			label: 'Portfolio Financing',
			desc: 'Structured across multiple investment properties.',
			tag: 'Investor Lending'
		}
	];

	onMount(() => {
		let cleanup: (() => void) | undefined;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			// Spotlight
			const xTo = gsap.quickTo(spotlightRef, 'x', {
				duration: 0.8,
				ease: 'power3.out'
			});

			const yTo = gsap.quickTo(spotlightRef, 'y', {
				duration: 0.8,
				ease: 'power3.out'
			});

			const moveSpotlight = (e: MouseEvent) => {
				const rect = sectionRef.getBoundingClientRect();

				xTo(e.clientX - rect.left - 250);
				yTo(e.clientY - rect.top - 250);
			};

			sectionRef.addEventListener('mousemove', moveSpotlight);

			const ctx = gsap.context(() => {
				// HEADLINE
				gsap.from(titleWords.filter(Boolean), {
					y: 120,
					rotateX: -90,
					opacity: 0,
					duration: 1.2,
					stagger: 0.05,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 82%'
					}
				});

				// PARAGRAPH
				gsap.from('.hero-copy', {
					y: 50,
					opacity: 0,
					duration: 1,
					delay: 0.3,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 80%'
					}
				});

				// STATS
				gsap.from(statsRefs.filter(Boolean), {
					y: 80,
					opacity: 0,
					stagger: 0.14,
					duration: 1.2,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: '.results-grid',
						start: 'top 88%'
					}
				});

				// FLOAT
				statsRefs.forEach((card, i) => {
					gsap.to(card, {
						y: i % 2 === 0 ? -10 : -18,
						duration: 4 + i,
						repeat: -1,
						yoyo: true,
						ease: 'sine.inOut'
					});
				});

				// IMAGE REVEAL
				gsap.from('.hero-image', {
					scale: 1.15,
					opacity: 0,
					duration: 1.5,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: '.hero-image',
						start: 'top 85%'
					}
				});

				// PARALLAX
				gsap.to('.mesh-1', {
					yPercent: -20,
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top bottom',
						end: 'bottom top',
						scrub: true
					}
				});

				gsap.to('.mesh-2', {
					yPercent: 18,
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top bottom',
						end: 'bottom top',
						scrub: true
					}
				});
			}, sectionRef);

			cleanup = () => {
				ctx.revert();
				sectionRef?.removeEventListener('mousemove', moveSpotlight);
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

	<div class="mesh mesh-1"></div>
	<div class="mesh mesh-2"></div>

	<div class="vignette"></div>

	<!-- SPOTLIGHT -->
	<div
		bind:this={spotlightRef}
		class="spotlight"
	></div>

	<!-- TOP LINE -->
	<div
		class="absolute inset-x-0 top-0 h-px bg-gradient-to-r from-transparent via-white/15 to-transparent"
	></div>

	<!-- SVG -->
	<svg
		class="pointer-events-none absolute inset-0 h-full w-full opacity-[0.08]"
		preserveAspectRatio="none"
	>
		<defs>
			<linearGradient id="results-line" x1="0%" y1="0%" x2="100%" y2="0%">
				<stop offset="0%" stop-color="#c5a059" stop-opacity="0" />
				<stop offset="50%" stop-color="#c5a059" stop-opacity="1" />
				<stop offset="100%" stop-color="#c5a059" stop-opacity="0" />
			</linearGradient>
		</defs>

		<path
			d="M0,160 C400,60 700,260 960,140 S1450,100 1920,220"
			stroke="url(#results-line)"
			stroke-width="1"
			fill="none"
		>
			<animate
				attributeName="d"
				dur="18s"
				repeatCount="indefinite"
				values="
        M0,160 C400,60 700,260 960,140 S1450,100 1920,220;
        M0,220 C350,300 850,40 960,100 S1500,120 1920,120;
        M0,160 C400,60 700,260 960,140 S1450,100 1920,220"
			/>
		</path>
	</svg>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<!-- HERO -->
		<div class="grid items-center gap-20 lg:grid-cols-[1.05fr_0.95fr]">
			<!-- LEFT -->
			<div>
				<div class="hero-badge">
					<div class="dot"></div>

					<span>
						Real Financial Outcomes. Structured Strategically.
					</span>
				</div>

				<h1
					class="mt-10 max-w-4xl font-serif text-[clamp(3.5rem,7vw,7rem)] leading-[0.92] tracking-[-0.05em] text-white"
					style="perspective:1000px;"
				>
					{#each 'Results That Change The Entire Financial Equation'.split(' ') as word, i}
						<span
							bind:this={titleWords[i]}
							class="mr-[0.22em] inline-block origin-bottom"
						>
							{word}
						</span>
					{/each}
				</h1>

				<p
					class="hero-copy mt-10 max-w-2xl text-lg leading-relaxed text-white/58 lg:text-xl"
				>
					These aren’t hypothetical calculators or generic mortgage scenarios.
					They’re structured outcomes designed to unlock equity, compress debt timelines,
					improve monthly cash flow, and create financial flexibility.
				</p>

				<div class="hero-actions">
					<a href="/strategy" class="btn-primary">
						<span>Explore Strategies</span>
					</a>

					<a href="/book-call" class="btn-secondary">
						<span>Book Strategy Call</span>
					</a>
				</div>
			</div>

			<!-- RIGHT -->
			<div class="relative">
				<div class="hero-image-wrap">
					<img
						class="hero-image"
						src={Hero}
						alt="Luxury financial strategy"
					/>

					<div class="image-overlay"></div>

					<div class="floating-result">
						<p class="floating-label">
							Average Cash Flow Improvement
						</p>

						<h3>+32%</h3>

						<div class="bar">
							<div class="bar-fill"></div>
						</div>
					</div>
				</div>
			</div>
		</div>

		<!-- RESULTS -->
		<div class="results-grid mt-28 grid gap-6 lg:grid-cols-3">
			{#each results as item, i}
				<div
					bind:this={statsRefs[i]}
					class="result-card"
				>
					<div class="card-noise"></div>

					<div class="result-top">
						<span>{item.tag}</span>
					</div>

					<h3>{item.number}</h3>

					<p class="result-label">
						{item.label}
					</p>

					<p class="result-desc">
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
		background-image: radial-gradient(rgba(255,255,255,0.3) 0.5px, transparent 0.5px);
		background-size: 4px 4px;
		pointer-events: none;
	}

	.mesh {
		position: absolute;
		border-radius: 9999px;
		filter: blur(120px);
	}

	.mesh-1 {
		top: -300px;
		left: -200px;
		width: 800px;
		height: 800px;
		background: rgba(197,160,89,0.11);
	}

	.mesh-2 {
		right: -300px;
		bottom: -250px;
		width: 900px;
		height: 900px;
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
		background: rgba(197,160,89,0.09);
		filter: blur(120px);
		pointer-events: none;
	}

	.hero-badge {
		display: inline-flex;
		align-items: center;
		gap: 12px;

		padding: 14px 18px;

		border-radius: 9999px;

		border: 1px solid rgba(255,255,255,0.08);

		background: rgba(255,255,255,0.03);

		backdrop-filter: blur(24px);
	}

	.hero-badge span {
		font-size: 11px;
		text-transform: uppercase;
		letter-spacing: 0.32em;
		color: rgba(255,255,255,0.55);
		font-family: monospace;
	}

	.dot {
		width: 8px;
		height: 8px;
		border-radius: 9999px;
		background: #c5a059;
		box-shadow: 0 0 18px rgba(197,160,89,0.8);
	}

	.hero-actions {
		display: flex;
		flex-wrap: wrap;
		gap: 18px;
		margin-top: 42px;
	}

	.btn-primary,
	.btn-secondary {
		position: relative;
		overflow: hidden;

		display: inline-flex;
		align-items: center;
		justify-content: center;

		padding: 16px 26px;

		border-radius: 9999px;

		font-size: 13px;
		font-weight: 600;
		letter-spacing: 0.08em;
		text-transform: uppercase;

		transition: all 0.45s ease;
	}

	.btn-primary {
		background: #c5a059;
		color: black;
	}

	.btn-primary:hover {
		transform: translateY(-3px);
		box-shadow: 0 20px 60px rgba(197,160,89,0.25);
	}

	.btn-secondary {
		border: 1px solid rgba(255,255,255,0.12);
		color: white;
		background: rgba(255,255,255,0.03);
		backdrop-filter: blur(20px);
	}

	.btn-secondary:hover {
		border-color: rgba(197,160,89,0.4);
		background: rgba(255,255,255,0.05);
	}

	.hero-image-wrap {
		position: relative;
		overflow: hidden;

		border-radius: 36px;

		border: 1px solid rgba(255,255,255,0.08);

		height: 760px;
	}

	.hero-image {
		width: 100%;
		height: 100%;
		object-fit: cover;
	}

	.image-overlay {
		position: absolute;
		inset: 0;

		background:
			linear-gradient(to top,
			rgba(0,0,0,0.88),
			transparent 40%);
	}

	.floating-result {
		position: absolute;
		left: 30px;
		right: 30px;
		bottom: 30px;

		padding: 28px;

		border-radius: 28px;

		border: 1px solid rgba(255,255,255,0.08);

		background: rgba(10,10,10,0.7);

		backdrop-filter: blur(24px);
	}

	.floating-label {
		font-size: 11px;
		text-transform: uppercase;
		letter-spacing: 0.28em;
		color: rgba(255,255,255,0.45);
		margin-bottom: 12px;
		font-family: monospace;
	}

	.floating-result h3 {
		font-size: 72px;
		line-height: 1;
		color: white;
		font-family: serif;
	}

	.bar {
		margin-top: 20px;
		height: 8px;
		border-radius: 9999px;
		background: rgba(255,255,255,0.08);
		overflow: hidden;
	}

	.bar-fill {
		width: 72%;
		height: 100%;
		background: linear-gradient(to right, #c5a059, #fff0c9);
		border-radius: inherit;
	}

	.results-grid {
		position: relative;
		z-index: 2;
	}

	.result-card {
		position: relative;
		overflow: hidden;

		padding: 34px;

		border-radius: 32px;

		border: 1px solid rgba(255,255,255,0.08);

		background: rgba(255,255,255,0.03);

		backdrop-filter: blur(24px);

		transition:
			transform 0.5s ease,
			border-color 0.5s ease,
			box-shadow 0.5s ease;
	}

	.result-card:hover {
		transform: translateY(-8px);
		border-color: rgba(197,160,89,0.28);
		box-shadow: 0 25px 70px rgba(197,160,89,0.12);
	}

	.card-noise {
		position: absolute;
		inset: 0;
		opacity: 0.03;
		background-image: radial-gradient(white 0.5px, transparent 0.5px);
		background-size: 5px 5px;
	}

	.result-top span {
		font-size: 10px;
		text-transform: uppercase;
		letter-spacing: 0.3em;
		color: #c5a059;
		font-family: monospace;
	}

	.result-card h3 {
		margin-top: 28px;

		font-size: clamp(3rem,5vw,5rem);
		line-height: 0.95;
		letter-spacing: -0.05em;

		color: white;
		font-family: serif;
	}

	.result-label {
		margin-top: 20px;

		font-size: 22px;
		color: white;
	}

	.result-desc {
		margin-top: 14px;

		font-size: 15px;
		line-height: 1.8;
		color: rgba(255,255,255,0.52);
	}

	@media (max-width: 1024px) {
		.hero-image-wrap {
			height: 560px;
		}
	}

	@media (max-width: 640px) {
		.hero-actions {
			flex-direction: column;
		}

		.btn-primary,
		.btn-secondary {
			width: 100%;
		}

		.hero-image-wrap {
			height: 460px;
		}

		.floating-result h3 {
			font-size: 52px;
		}
	}
</style>