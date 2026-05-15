<script lang="ts">
	import { onMount } from 'svelte';
    import Reaction from '$lib/assets/results/5.webp';

	let sectionRef: HTMLElement;
	let headingRef: HTMLElement;
	let quoteRef: HTMLElement;
	let imageRef: HTMLElement;

	const reactions = [
		'“I didn’t know this was even possible.”',
		'“Nobody explained equity like this before.”',
		'“I thought refinancing was my only option.”',
		'“This completely changed how I think about debt.”'
	];

	onMount(() => {
		let cleanup: (() => void) | undefined;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			const ctx = gsap.context(() => {
				gsap.from(headingRef, {
					y: 100,
					opacity: 0,
					duration: 1.2,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: headingRef,
						start: 'top 82%'
					}
				});

				gsap.from(quoteRef.querySelectorAll('.quote-row'), {
					x: -70,
					opacity: 0,
					stagger: 0.15,
					duration: 1,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: quoteRef,
						start: 'top 78%'
					}
				});

				gsap.from(imageRef, {
					scale: 1.12,
					opacity: 0,
					duration: 1.5,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: imageRef,
						start: 'top 75%'
					}
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
	class="relative overflow-hidden bg-[#080808] py-32 lg:py-44"
>
	<!-- BG -->
	<div class="noise"></div>

	<div class="gradient-orb orb-1"></div>
	<div class="gradient-orb orb-2"></div>

	<div class="vignette"></div>

	<!-- GRID -->
	<div class="grid-lines"></div>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<!-- TOP -->
		<div
			bind:this={headingRef}
			class="mb-24 flex flex-col gap-12 lg:flex-row lg:items-end lg:justify-between"
		>
			<div class="max-w-4xl">
				<div
					class="mb-7 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl"
				>
					<div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

					<span class="font-mono text-[11px] uppercase tracking-[0.35em] text-white/45">
						The Reaction We Hear Constantly
					</span>
				</div>

				<h2
					class="font-serif text-[clamp(3rem,6vw,6rem)] leading-[0.94] tracking-[-0.05em] text-white"
				>
					Most People Aren’t
					<span class="italic text-[#c5a059]"> Under-Earning.</span>

					<br />

					They’re Simply
					<span class="italic text-[#c5a059]"> Structured Wrong.</span>
				</h2>
			</div>

			<p class="max-w-lg text-lg leading-relaxed text-white/52">
				Once clients understand how interest, equity, and cash flow actually work together,
				their entire financial perspective changes.
			</p>
		</div>

		<!-- MAIN -->
		<div class="grid grid-cols-1 gap-14 lg:grid-cols-[0.95fr_1.05fr]">
			<!-- LEFT -->
			<div
				bind:this={quoteRef}
				class="flex flex-col justify-center gap-5"
			>
				{#each reactions as quote}
					<div class="quote-row">
						<div class="quote-glow"></div>

						<div class="relative z-10 flex items-center gap-6">
							<div class="quote-dot"></div>

							<h3
								class="font-serif text-[clamp(1.6rem,3vw,2.6rem)] leading-[1.1] tracking-[-0.03em] text-white"
							>
								{quote}
							</h3>
						</div>
					</div>
				{/each}
			</div>

			<!-- RIGHT -->
			<div bind:this={imageRef} class="relative overflow-hidden rounded-[40px]">
				<img
					src={Reaction}
					alt=""
					class="h-[760px] w-full object-cover"
				/>

				<!-- OVERLAY -->
				<div class="image-overlay"></div>

				<!-- FLOATING CONTENT -->
				<div class="floating-panel">
					<p
						class="mb-4 font-mono text-[11px] uppercase tracking-[0.3em] text-[#c5a059]"
					>
						The Shift
					</p>

					<h3
						class="max-w-md font-serif text-4xl leading-[1.02] tracking-[-0.04em] text-white"
					>
						From Loan Shopping
						To Strategic Capital Control.
					</h3>

					<div class="mt-8 h-px w-full bg-gradient-to-r from-[#c5a059]/40 to-transparent"></div>

					<p class="mt-8 text-[15px] leading-relaxed text-white/58">
						The objective isn’t finding the “lowest payment.”
						The objective is creating flexibility, preserving liquidity,
						and accelerating long-term wealth positioning.
					</p>
				</div>
			</div>
		</div>
	</div>
</section>

<style>
	:global(body) {
		background: #080808;
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

	.grid-lines {
		position: absolute;
		inset: 0;

		background-image:
			linear-gradient(rgba(255,255,255,0.025) 1px, transparent 1px),
			linear-gradient(90deg, rgba(255,255,255,0.025) 1px, transparent 1px);

		background-size: 140px 140px;

		mask-image: radial-gradient(circle at center, black, transparent 90%);
	}

	.vignette {
		position: absolute;
		inset: 0;

		background:
			radial-gradient(circle at center,
			transparent 25%,
			rgba(0,0,0,0.84) 100%);
	}

	.gradient-orb {
		position: absolute;
		border-radius: 9999px;
		filter: blur(140px);
	}

	.orb-1 {
		left: -250px;
		top: -200px;

		width: 800px;
		height: 800px;

		background: rgba(197,160,89,0.08);
	}

	.orb-2 {
		right: -300px;
		bottom: -250px;

		width: 900px;
		height: 900px;

		background: rgba(197,160,89,0.06);
	}

	.quote-row {
		position: relative;
		overflow: hidden;

		padding: 34px 36px;

		border-radius: 30px;

		border: 1px solid rgba(255,255,255,0.08);

		background:
			linear-gradient(
				180deg,
				rgba(255,255,255,0.04),
				rgba(255,255,255,0.02)
			);

		backdrop-filter: blur(20px);
	}

	.quote-glow {
		position: absolute;
		inset: 0;

		background:
			radial-gradient(circle at left,
			rgba(197,160,89,0.14),
			transparent 65%);

		opacity: 0;

		transition: opacity 0.5s ease;
	}

	.quote-row:hover .quote-glow {
		opacity: 1;
	}

	.quote-dot {
		width: 12px;
		height: 12px;

		border-radius: 9999px;

		background: #c5a059;

		box-shadow:
			0 0 25px rgba(197,160,89,0.5);

		flex-shrink: 0;
	}

	.image-overlay {
		position: absolute;
		inset: 0;

		background:
			linear-gradient(
				to top,
				rgba(0,0,0,0.82),
				rgba(0,0,0,0.18)
			);
	}

	.floating-panel {
		position: absolute;
		left: 40px;
		right: 40px;
		bottom: 40px;

		padding: 36px;

		border-radius: 32px;

		border: 1px solid rgba(255,255,255,0.08);

		background: rgba(8,8,8,0.72);

		backdrop-filter: blur(24px);

		box-shadow:
			0 30px 100px rgba(0,0,0,0.45);
	}

	@media (max-width: 768px) {
		.floating-panel {
			left: 20px;
			right: 20px;
			bottom: 20px;

			padding: 24px;
		}
	}
</style>