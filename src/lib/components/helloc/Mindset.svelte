<script lang="ts">
	import { onMount } from 'svelte';
  import Mind from '$lib/assets/helloc/2.webp';

	let sectionRef: HTMLElement;
	let imageRef: HTMLElement;
	let contentRef: HTMLElement;
	let quoteRef: HTMLElement;

	const comparisons = [
		{
			old: 'Traditional Mortgage',
			new: 'First Lien HELOC',
			points: [
				'Fixed monthly amortization',
				'Interest front-loaded for years',
				'Money sits idle in checking',
				'Little control over payoff speed'
			]
		},
		{
			old: 'Conventional Thinking',
			new: 'Strategic Cash Flow',
			points: [
				'Income disappears into expenses',
				'Debt controls the timeline',
				'Banks dictate the structure',
				'Cash flow becomes the weapon'
			]
		}
	];

	onMount(() => {
		let ctx: any;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			ctx = gsap.context(() => {
				gsap.from(contentRef, {
					y: 100,
					opacity: 0,
					duration: 1.4,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 75%'
					}
				});

				gsap.from(imageRef, {
					scale: 0.82,
					opacity: 0,
					rotate: -4,
					duration: 1.8,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: imageRef,
						start: 'top 85%'
					}
				});

				gsap.from('.compare-row', {
					y: 50,
					opacity: 0,
					stagger: 0.15,
					duration: 1,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: '.compare-grid',
						start: 'top 85%'
					}
				});

				gsap.from(quoteRef, {
					y: 60,
					opacity: 0,
					scale: 0.95,
					duration: 1.2,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: quoteRef,
						start: 'top 90%'
					}
				});

				// floating image
				gsap.to(imageRef, {
					y: -14,
					duration: 5,
					repeat: -1,
					yoyo: true,
					ease: 'sine.inOut'
				});
			}, sectionRef);
		};

		init();

		return () => {
			ctx?.revert();
		};
	});
</script>

<section
	bind:this={sectionRef}
	class="relative overflow-hidden bg-[var(--theme-color)] py-32 lg:py-44"
>
	<!-- BACKGROUND -->
	<div class="noise"></div>

	<div class="gold-orb"></div>

	<div class="vignette"></div>

	<!-- TOP LINE -->
	<div
		class="absolute inset-x-0 top-0 h-px bg-gradient-to-r from-transparent via-white/10 to-transparent"
	></div>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<!-- HEADER -->
		<div bind:this={contentRef} class="max-w-4xl">
			<div
				class="mb-7 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl"
			>
				<div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

				<span class="font-mono text-[11px] uppercase tracking-[0.34em] text-white/50">
					The Financial Mindset Shift
				</span>
			</div>

			<h2
				class="font-serif text-[clamp(3rem,5vw,5.8rem)] leading-[0.95] tracking-[-0.04em] text-white"
			>
				The Goal Isn’t
				<span class="italic text-[#c5a059]">
					A Lower Payment.
				</span>

				It’s Financial Control.
			</h2>

			<p class="mt-8 max-w-3xl text-lg leading-relaxed text-white/58 lg:text-[1.16rem]">
				Most homeowners optimize for the monthly payment.
				High-level financial strategy optimizes for liquidity, velocity,
				interest efficiency, and control over capital.
			</p>
		</div>

		<!-- CENTER VISUAL -->
		<div class="relative mt-24">
			<div
				bind:this={imageRef}
				class="image-wrap"
			>
				<img
					src={Mind}
					alt="Financial strategy"
					class="h-full w-full object-cover"
				/>

				<div class="image-overlay"></div>

				<div class="image-content">
					<p class="mini-label">
						What Changes
					</p>

					<h3>
						You stop thinking
						about payments…
					</h3>

					<h4>
						and start thinking
						about leverage.
					</h4>
				</div>
			</div>

			<!-- FLOATING QUOTE -->
			<div bind:this={quoteRef} class="quote-box">
				<div class="quote-line"></div>

				<p>
					“Most people never lose because of the interest rate.
					They lose because they never learned how money flows.”
				</p>
			</div>
		</div>

		<!-- COMPARISON -->
		<div class="compare-grid mt-28">
			{#each comparisons as item}
				<div class="compare-row">
					<div class="compare-head">
						<div>
							<p class="tag">
								Old Model
							</p>

							<h3 class="faded">
								{item.old}
							</h3>
						</div>

						<div class="arrow">
							→
						</div>

						<div>
							<p class="tag gold">
								New Strategy
							</p>

							<h3 class="gold-text">
								{item.new}
							</h3>
						</div>
					</div>

					<div class="points">
						{#each item.points as point}
							<div class="point">
								<div class="dot"></div>

								<p>{point}</p>
							</div>
						{/each}
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

	.gold-orb {
		position: absolute;
		right: -200px;
		top: 10%;
		width: 700px;
		height: 700px;
		border-radius: 9999px;
		background: rgba(197,160,89,0.08);
		filter: blur(150px);
	}

	.image-wrap {
		position: relative;
		overflow: hidden;

		border-radius: 42px;

		height: 700px;

		border: 1px solid rgba(255,255,255,0.08);

		box-shadow:
			0 40px 120px rgba(0,0,0,0.5);
	}

	.image-overlay {
		position: absolute;
		inset: 0;

		background:
			linear-gradient(
				180deg,
				rgba(0,0,0,0.1),
				rgba(0,0,0,0.75)
			);
	}

	.image-content {
		position: absolute;
		left: 60px;
		bottom: 60px;
		max-width: 700px;
		z-index: 2;
	}

	.mini-label {
		font-size: 11px;
		letter-spacing: 0.28em;
		text-transform: uppercase;
		font-family: monospace;
		color: rgba(255,255,255,0.45);
		margin-bottom: 22px;
	}

	.image-content h3 {
		font-family: serif;
		font-size: clamp(2.4rem, 5vw, 5rem);
		line-height: 0.95;
		color: white;
		letter-spacing: -0.04em;
	}

	.image-content h4 {
		margin-top: 14px;

		font-family: serif;
		font-style: italic;

		font-size: clamp(2rem, 4vw, 4.5rem);

		line-height: 1;

		color: #c5a059;
		letter-spacing: -0.04em;
	}

	.quote-box {
		position: absolute;
		right: 40px;
		bottom: -60px;

		max-width: 420px;

		padding: 34px;

		border-radius: 30px;

		border: 1px solid rgba(255,255,255,0.08);

		background:
			linear-gradient(
				180deg,
				rgba(255,255,255,0.06),
				rgba(255,255,255,0.03)
			);

		backdrop-filter: blur(24px);

		box-shadow:
			0 20px 60px rgba(0,0,0,0.45);
	}

	.quote-line {
		width: 70px;
		height: 1px;
		background: #c5a059;
		margin-bottom: 22px;
	}

	.quote-box p {
		font-size: 1.15rem;
		line-height: 1.9;
		color: rgba(255,255,255,0.8);
	}

	.compare-grid {
		display: flex;
		flex-direction: column;
		gap: 40px;
	}

	.compare-row {
		padding: 42px;

		border-radius: 36px;

		border: 1px solid rgba(255,255,255,0.08);

		background:
			linear-gradient(
				180deg,
				rgba(255,255,255,0.04),
				rgba(255,255,255,0.02)
			);

		backdrop-filter: blur(20px);
	}

	.compare-head {
		display: flex;
		align-items: center;
		justify-content: space-between;
		gap: 30px;

		padding-bottom: 28px;
		margin-bottom: 30px;

		border-bottom: 1px solid rgba(255,255,255,0.08);
	}

	.tag {
		font-size: 10px;
		letter-spacing: 0.24em;
		text-transform: uppercase;
		font-family: monospace;
		color: rgba(255,255,255,0.35);
		margin-bottom: 12px;
	}

	.tag.gold {
		color: rgba(197,160,89,0.7);
	}

	.faded {
		font-family: serif;
		font-size: clamp(1.7rem, 3vw, 3rem);
		color: rgba(255,255,255,0.38);
	}

	.gold-text {
		font-family: serif;
		font-size: clamp(1.7rem, 3vw, 3rem);
		color: #c5a059;
		font-style: italic;
	}

	.arrow {
		font-size: 2rem;
		color: rgba(255,255,255,0.25);
	}

	.points {
		display: grid;
		grid-template-columns: repeat(2, minmax(0,1fr));
		gap: 24px;
	}

	.point {
		display: flex;
		align-items: flex-start;
		gap: 14px;
	}

	.dot {
		width: 8px;
		height: 8px;
		margin-top: 10px;
		border-radius: 9999px;
		background: #c5a059;
		flex-shrink: 0;
		box-shadow: 0 0 20px rgba(197,160,89,0.4);
	}

	.point p {
		font-size: 15px;
		line-height: 1.9;
		color: rgba(255,255,255,0.58);
	}

	@media (max-width: 768px) {
		.image-wrap {
			height: 520px;
		}

		.image-content {
			left: 30px;
			right: 30px;
			bottom: 30px;
		}

		.quote-box {
			position: relative;
			right: auto;
			bottom: auto;
			margin-top: 24px;
			max-width: 100%;
		}

		.compare-head {
			flex-direction: column;
			align-items: flex-start;
		}

		.points {
			grid-template-columns: 1fr;
		}
	}
</style>