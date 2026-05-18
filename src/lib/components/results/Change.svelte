<script lang="ts">
	import { onMount } from 'svelte';

    import Change from "$lib/assets/results/4.webp"

	let sectionRef: HTMLElement;
	let titleRef: HTMLElement;
	let imageRef: HTMLElement;
	let contentRef: HTMLElement;
	let metricRefs: HTMLElement[] = [];

	const metrics = [
		{
			value: '7 Years',
			label: 'Accelerated Mortgage Payoff',
			desc: 'Without refinancing into a higher rate.'
		},
		{
			value: '43%',
			label: 'Average Interest Reduction',
			desc: 'Through cash flow optimization strategies.'
		},
		{
			value: '$1.8M+',
			label: 'Equity Repositioned',
			desc: 'Across custom HELOC and reverse structures.'
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
					y: 100,
					opacity: 0,
					duration: 1.2,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: titleRef,
						start: 'top 85%'
					}
				});

				gsap.from(imageRef, {
					scale: 1.15,
					opacity: 0,
					duration: 1.5,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: imageRef,
						start: 'top 75%'
					}
				});

				gsap.from(contentRef, {
					x: 90,
					opacity: 0,
					duration: 1.2,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: contentRef,
						start: 'top 80%'
					}
				});

				gsap.from(metricRefs, {
					y: 50,
					opacity: 0,
					stagger: 0.12,
					duration: 0.9,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: contentRef,
						start: 'top 82%'
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
	class="relative overflow-hidden bg-[var(--theme-color)] py-32 lg:py-44"
>
	<!-- BG -->
	<div class="noise"></div>

	<div class="mesh mesh-1"></div>
	<div class="mesh mesh-2"></div>

	<div class="vignette"></div>

	<!-- GOLD LINE -->
	<div
		class="absolute inset-x-0 top-0 h-px bg-gradient-to-r from-transparent via-[#c5a059]/40 to-transparent"
	></div>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<!-- TITLE -->
		<div bind:this={titleRef} class="mb-24 text-center">
			<div
				class="mb-7 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl"
			>
				<div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

				<span class="font-mono text-[11px] uppercase tracking-[0.35em] text-white/45">
					The Math Changes Everything
				</span>
			</div>

			<h2
				class="mx-auto max-w-5xl font-serif text-[clamp(3rem,6vw,6rem)] leading-[0.94] tracking-[-0.05em] text-white"
			>
				The Goal Wasn’t
				<span class="italic text-[#c5a059]"> A Lower Rate.</span>

				<br />

				The Goal Was
				<span class="italic text-[#c5a059]"> Faster Financial Freedom.</span>
			</h2>
		</div>

		<!-- MAIN -->
		<div class="grid grid-cols-1 items-center gap-20 lg:grid-cols-[1.05fr_0.95fr]">
			<!-- IMAGE -->
			<div bind:this={imageRef} class="relative">
				<div class="image-wrap">
					<img
						src={Change}
						alt=""
						class="h-[720px] w-full object-cover"
					/>

					<div class="image-overlay"></div>
				</div>

				<!-- FLOATING RESULT -->
				<div class="floating-stat floating-1">
					<p class="mb-2 text-[11px] uppercase tracking-[0.28em] text-white/35">
						Interest Saved
					</p>

					<h3 class="font-serif text-4xl text-white">$412K</h3>
				</div>

				<div class="floating-stat floating-2">
					<p class="mb-2 text-[11px] uppercase tracking-[0.28em] text-white/35">
						Time Reduced
					</p>

					<h3 class="font-serif text-4xl text-white">23 Years</h3>
				</div>
			</div>

			<!-- CONTENT -->
			<div bind:this={contentRef}>
				<p
					class="mb-8 font-mono text-[11px] uppercase tracking-[0.35em] text-[#c5a059]"
				>
					Case Study Breakdown
				</p>

				<h3
					class="max-w-xl font-serif text-[clamp(2.2rem,4vw,4.3rem)] leading-[1] tracking-[-0.04em] text-white"
				>
					A Traditional Mortgage Was Quietly
					<span class="italic text-[#c5a059]"> Destroying Cash Flow.</span>
				</h3>

				<div class="mt-10 space-y-7 text-[17px] leading-relaxed text-white/58">
					<p>
						The client originally believed refinancing was the only option.
						But replacing a 3% mortgage with a 7% loan would have increased
						long-term interest dramatically.
					</p>

					<p>
						Instead, we built a First Lien HELOC structure that redirected
						monthly income strategically, accelerated principal reduction,
						and dramatically shortened the payoff timeline.
					</p>

					<p>
						Same income. Different structure.
						Completely different outcome.
					</p>
				</div>

				<!-- METRICS -->
				<div class="mt-14 space-y-5">
					{#each metrics as metric, i}
						<div
							bind:this={metricRefs[i]}
							class="metric-row"
						>
							<div>
								<h4
									class="font-serif text-[2.4rem] leading-none tracking-[-0.04em] text-white"
								>
									{metric.value}
								</h4>
							</div>

							<div class="flex-1 border-t border-white/10"></div>

							<div class="max-w-[320px]">
								<p class="mb-1 text-sm font-medium text-white">
									{metric.label}
								</p>

								<p class="text-sm leading-relaxed text-white/45">
									{metric.desc}
								</p>
							</div>
						</div>
					{/each}
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

		opacity: 0.025;

		pointer-events: none;

		background-image:
			radial-gradient(rgba(255,255,255,0.35) 0.5px, transparent 0.5px);

		background-size: 4px 4px;
	}

	.vignette {
		position: absolute;
		inset: 0;

		background:
			radial-gradient(circle at center,
			transparent 20%,
			rgba(0,0,0,0.85) 100%);
	}

	.mesh {
		position: absolute;
		border-radius: 9999px;
		filter: blur(140px);
	}

	.mesh-1 {
		left: -250px;
		top: -250px;

		width: 800px;
		height: 800px;

		background: rgba(197,160,89,0.08);
	}

	.mesh-2 {
		right: -350px;
		bottom: -350px;

		width: 900px;
		height: 900px;

		background: rgba(197,160,89,0.06);
	}

	.image-wrap {
		position: relative;
		overflow: hidden;

		border-radius: 38px;
	}

	.image-overlay {
		position: absolute;
		inset: 0;

		background:
			linear-gradient(
				to top,
				rgba(0,0,0,0.45),
				transparent 40%
			);
	}

	.floating-stat {
		position: absolute;

		padding: 22px 24px;

		border-radius: 24px;

		border: 1px solid rgba(255,255,255,0.1);

		background: rgba(8,8,8,0.75);

		backdrop-filter: blur(20px);

		box-shadow:
			0 20px 80px rgba(0,0,0,0.45);
	}

	.floating-1 {
		left: -40px;
		top: 80px;
	}

	.floating-2 {
		right: -40px;
		bottom: 80px;
	}

	.metric-row {
		display: flex;
		align-items: center;
		gap: 26px;
	}

	@media (max-width: 768px) {
		.metric-row {
			flex-direction: column;
			align-items: flex-start;
		}

		.floating-1,
		.floating-2 {
			position: relative;
			left: auto;
			right: auto;
			top: auto;
			bottom: auto;

			margin-top: 20px;
		}
	}
</style>