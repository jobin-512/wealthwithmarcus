<script lang="ts">
	import { onMount } from 'svelte';
    import Home from "$lib/assets/stratergy/1.webp"

	let sectionRef: HTMLElement;
	let imageWrap: HTMLElement;
	let imageRef: HTMLElement;
	let contentRef: HTMLElement;
	let quoteRef: HTMLElement;
	let spotlightRef: HTMLElement;

	onMount(() => {
		let cleanup: (() => void) | undefined;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			// Spotlight
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
				// Image reveal
				gsap.from(imageWrap, {
					scale: 0.92,
					opacity: 0,
					duration: 1.6,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: imageWrap,
						start: 'top 80%'
					}
				});

				// Image parallax
				gsap.to(imageRef, {
					yPercent: -12,
					ease: 'none',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top bottom',
						end: 'bottom top',
						scrub: true
					}
				});

				// Content reveal
				gsap.from(contentRef.children, {
					y: 90,
					opacity: 0,
					stagger: 0.14,
					duration: 1.1,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: contentRef,
						start: 'top 80%'
					}
				});

				// Quote float
				gsap.to(quoteRef, {
					y: -12,
					duration: 4,
					repeat: -1,
					yoyo: true,
					ease: 'sine.inOut'
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

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<div class="grid items-center gap-20 lg:grid-cols-[1.05fr_0.95fr]">
			<!-- IMAGE SIDE -->
			<div
				bind:this={imageWrap}
				class="image-wrap"
			>
				<!-- IMAGE -->
				<div class="image-mask">
					<img
						bind:this={imageRef}
						src={Home}
						alt="Luxury modern home"
						class="image"
					/>
				</div>

				<!-- OVERLAY -->
				<div class="image-overlay"></div>

				<!-- GRAIN -->
				<div class="grain"></div>

				<!-- FLOATING QUOTE -->
				<div
					bind:this={quoteRef}
					class="quote-card"
				>
					<div class="quote-line"></div>

					<p>
						Most homeowners don’t need
						a lower rate.
					</p>

					<h4>
						They need a better system.
					</h4>
				</div>
			</div>

			<!-- CONTENT -->
			<div
				bind:this={contentRef}
				class="content"
			>
				<div class="eyebrow">
					<div class="dot"></div>

					<span>
						Equity Strategy
					</span>
				</div>

				<h2 class="headline">
					Your Home
					<span class="accent">Should Be</span>

					<br />

					A Financial Tool —
					<span class="accent">Not A Trap.</span>
				</h2>

				<p class="body">
					The traditional mortgage system was built
					to maximize long-term interest collection —
					not accelerate your financial freedom.
				</p>

				<p class="body muted">
					Most homeowners unknowingly lock themselves into
					decades of inefficient cash flow while their equity
					sits dormant.
				</p>

				<div class="divider"></div>

				<div class="bottom-copy">
					<p>
						Strategic lending changes the equation.
					</p>

					<p>
						When structured correctly,
						your income, equity, and cash flow
						begin working together instead of against you.
					</p>
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
		pointer-events: none;

		background-image:
			radial-gradient(rgba(255,255,255,0.35) 0.5px, transparent 0.5px);

		background-size: 4px 4px;
	}

	.mesh {
		position: absolute;
		border-radius: 9999px;
		filter: blur(120px);
	}

	.mesh-1 {
		left: -200px;
		top: -200px;

		width: 700px;
		height: 700px;

		background: rgba(197,160,89,0.12);
	}

	.mesh-2 {
		right: -240px;
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

	.image-wrap {
		position: relative;
	}

	.image-mask {
		position: relative;
		overflow: hidden;

		border-radius: 40px;

		height: 760px;

		border: 1px solid rgba(255,255,255,0.08);
	}

	.image {
		width: 100%;
		height: 120%;

		object-fit: cover;

		filter:
			saturate(0.88)
			contrast(1.05)
			brightness(0.82);
	}

	.image-overlay {
		position: absolute;
		inset: 0;

		border-radius: 40px;

		background:
			linear-gradient(
				to top,
				rgba(0,0,0,0.82),
				transparent 40%
			);
	}

	.grain {
		position: absolute;
		inset: 0;

		border-radius: 40px;

		background-image:
			radial-gradient(rgba(255,255,255,0.12) 0.5px, transparent 0.5px);

		background-size: 4px 4px;

		opacity: 0.08;

		pointer-events: none;
	}

	.quote-card {
		position: absolute;
		right: -30px;
		bottom: 60px;

		max-width: 320px;

		padding: 28px;

		border-radius: 28px;

		border: 1px solid rgba(255,255,255,0.08);

		background: rgba(255,255,255,0.05);

		backdrop-filter: blur(20px);

		box-shadow:
			0 0 80px rgba(0,0,0,0.35);
	}

	.quote-line {
		width: 70px;
		height: 1px;

		margin-bottom: 18px;

		background: linear-gradient(
			90deg,
			#c5a059,
			transparent
		);
	}

	.quote-card p {
		font-size: 1rem;
		line-height: 1.8;

		color: rgba(255,255,255,0.58);
	}

	.quote-card h4 {
		margin-top: 10px;

		font-size: 1.6rem;
		line-height: 1.3;

		font-family: serif;

		color: white;
	}

	.eyebrow {
		display: inline-flex;
		align-items: center;
		gap: 14px;

		padding: 14px 20px;

		border-radius: 9999px;

		border: 1px solid rgba(255,255,255,0.08);

		background: rgba(255,255,255,0.03);

		backdrop-filter: blur(12px);
	}

	.dot {
		width: 8px;
		height: 8px;

		border-radius: 9999px;

		background: #c5a059;

		box-shadow: 0 0 20px #c5a059;
	}

	.eyebrow span {
		font-size: 11px;
		letter-spacing: 0.3em;
		text-transform: uppercase;
		color: rgba(255,255,255,0.5);
		font-family: monospace;
	}

	.headline {
		margin-top: 34px;

		font-size: clamp(3rem, 5vw, 5.8rem);

		line-height: 0.95;

		letter-spacing: -0.05em;

		font-family: serif;

		color: white;
	}

	.accent {
		color: #c5a059;
		font-style: italic;
	}

	.body {
		margin-top: 34px;

		font-size: 1.1rem;
		line-height: 2;

		color: rgba(255,255,255,0.62);
	}

	.body.muted {
		margin-top: 18px;
		color: rgba(255,255,255,0.46);
	}

	.divider {
		width: 100%;
		height: 1px;

		margin: 42px 0;

		background:
			linear-gradient(
				90deg,
				rgba(255,255,255,0.12),
				transparent
			);
	}

	.bottom-copy {
		display: flex;
		flex-direction: column;
		gap: 22px;
	}

	.bottom-copy p:first-child {
		font-size: 1.4rem;
		font-family: serif;
		color: white;
	}

	.bottom-copy p:last-child {
		font-size: 1rem;
		line-height: 1.9;
		color: rgba(255,255,255,0.5);
		max-width: 560px;
	}

	@media (max-width: 1024px) {
		.image-mask {
			height: 620px;
		}

		.quote-card {
			right: 20px;
			left: 20px;
			bottom: 20px;
			max-width: unset;
		}
	}

	@media (max-width: 768px) {
		.image-mask {
			height: 520px;
			border-radius: 28px;
		}

		.quote-card {
			padding: 22px;
		}

		.headline {
			font-size: 3rem;
		}
	}
</style>