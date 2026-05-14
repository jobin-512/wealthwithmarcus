<script lang="ts">
	import { onMount } from 'svelte';

	let sectionRef: HTMLElement;
	let titleRef: HTMLElement;
	let timelineRef: HTMLElement;
	let lineRef: HTMLElement;
	let steps: HTMLElement[] = $state([]);

	const process = [
		{
			number: '01',
			title: 'Financial Position Analysis',
			text:
				'We analyze your current mortgage structure, liquidity position, income flow, and long-term objectives to uncover inefficiencies traditional lenders ignore.'
		},
		{
			number: '02',
			title: 'Capital Strategy Design',
			text:
				'We architect a customized lending and equity strategy designed around cash flow optimization, equity access, and accelerated financial flexibility.'
		},
		{
			number: '03',
			title: 'Execution & Structuring',
			text:
				'Our team aligns the right lending products, structures the approvals, and simplifies the process into a streamlined execution path.'
		},
		{
			number: '04',
			title: 'Long-Term Optimization',
			text:
				'We continue refining the strategy as your income, assets, equity, and goals evolve — keeping your capital working intelligently.'
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
						start: 'top 82%'
					}
				});

				gsap.from(lineRef, {
					scaleY: 0,
					transformOrigin: 'top center',
					duration: 1.5,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: timelineRef,
						start: 'top 78%'
					}
				});

				steps.forEach((step, i) => {
					gsap.from(step, {
						x: i % 2 === 0 ? -100 : 100,
						opacity: 0,
						duration: 1.2,
						delay: i * 0.12,
						ease: 'power4.out',
						scrollTrigger: {
							trigger: step,
							start: 'top 88%'
						}
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
	class="relative overflow-hidden bg-[#050505] py-32 lg:py-44"
>
	<!-- background -->
	<div class="noise"></div>

	<div class="gradient-orb orb-1"></div>
	<div class="gradient-orb orb-2"></div>

	<div class="vignette"></div>

	<!-- top divider -->
	<div
		class="absolute inset-x-0 top-0 h-px bg-gradient-to-r from-transparent via-white/10 to-transparent"
	></div>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<!-- heading -->
		<div
			bind:this={titleRef}
			class="mb-28 grid gap-10 lg:grid-cols-[1.1fr_0.9fr] lg:items-end"
		>
			<div>
				<div class="eyebrow">
					<div class="dot"></div>

					<span>Strategic Process</span>
				</div>

				<h2
					class="mt-8 font-serif text-[clamp(3rem,6vw,6rem)] leading-[0.95] tracking-[-0.04em] text-white"
				>
					This Isn’t
					<br />
					Loan Shopping.
				</h2>
			</div>

			<p class="max-w-xl text-lg leading-relaxed text-white/50 lg:ml-auto">
				Most lenders focus on transactions.
				We focus on financial structure.
				The process is designed to uncover leverage points, eliminate friction,
				and position your capital more intelligently over time.
			</p>
		</div>

		<!-- timeline -->
		<div
			bind:this={timelineRef}
			class="relative mx-auto max-w-6xl"
		>
			<!-- vertical line -->
			<div
				bind:this={lineRef}
				class="timeline-line hidden md:block"
			></div>

			<div class="space-y-24 lg:space-y-32">
				{#each process as step, i}
					<div
						bind:this={steps[i]}
						class="timeline-row"
						class:reverse={i % 2 !== 0}
					>
						<!-- number -->
						<div class="timeline-number-wrap">
							<div class="timeline-number">
								{step.number}
							</div>
						</div>

						<!-- content -->
						<div class="timeline-content">
							<div class="content-line"></div>

							<p class="mini-label">
								Phase {step.number}
							</p>

							<h3>
								{step.title}
							</h3>

							<p class="desc">
								{step.text}
							</p>
						</div>
					</div>
				{/each}
			</div>
		</div>

		<!-- bottom statement -->
		<div class="bottom-statement">
			<div class="statement-line"></div>

			<p>
				“Most people optimize for rates.
				<br />
				We optimize for control.”
			</p>
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

	.gradient-orb {
		position: absolute;
		border-radius: 9999px;
		filter: blur(140px);
	}

	.orb-1 {
		top: -200px;
		left: -180px;

		width: 700px;
		height: 700px;

		background: rgba(197,160,89,0.08);
	}

	.orb-2 {
		right: -200px;
		bottom: -250px;

		width: 700px;
		height: 700px;

		background: rgba(197,160,89,0.06);
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

	.timeline-line {
		position: absolute;
		left: 50%;
		top: 0;
		bottom: 0;

		width: 1px;

		background:
			linear-gradient(
				to bottom,
				transparent,
				rgba(197,160,89,0.6),
				transparent
			);

		transform: translateX(-50%);
	}

.timeline-row {
	position: relative;

	display: grid;
	grid-template-columns: 1fr 140px 1fr;
	align-items: center;
	column-gap: 40px;
}

.timeline-row.reverse .timeline-content {
	grid-column: 1 / 2;
	justify-self: end;
	text-align: right;
}

.timeline-row:not(.reverse) .timeline-content {
	grid-column: 3 / 4;
	justify-self: start;
	text-align: left;
}

.timeline-row.reverse .timeline-number-wrap {
	grid-column: 2 / 3;
}

.timeline-row:not(.reverse) .timeline-number-wrap {
	grid-column: 2 / 3;
}

.timeline-number-wrap {
	position: relative;
	z-index: 5;

	display: flex;
	align-items: center;
	justify-content: center;
}

.timeline-content {
	position: relative;
	width: min(100%, 520px);
}

.timeline-row.reverse .content-line {
	right: -100px;
	left: auto;
}

.timeline-row:not(.reverse) .content-line {
	left: -100px;
	right: auto;
}

	.timeline-number-wrap {
		position: relative;
		z-index: 5;

		display: flex;
		align-items: center;
		justify-content: center;
	}

	.timeline-number {
		width: 110px;
		height: 110px;

		display: flex;
		align-items: center;
		justify-content: center;

		border-radius: 9999px;

		border: 1px solid rgba(197,160,89,0.18);

		background:
			radial-gradient(circle at top,
			rgba(197,160,89,0.14),
			rgba(255,255,255,0.02));

		backdrop-filter: blur(24px);

		font-size: 30px;
		font-family: serif;

		color: white;

		box-shadow:
			0 0 60px rgba(197,160,89,0.08);
	}

	.timeline-content {
		position: relative;
		max-width: 460px;
	}

	.content-line {
		position: absolute;
		left: -100px;
		top: 52px;

		width: 80px;
		height: 1px;

		background:
			linear-gradient(
				to right,
				rgba(197,160,89,0.8),
				transparent
			);
	}

	.mini-label {
		margin-bottom: 18px;

		font-size: 10px;
		letter-spacing: 0.3em;
		text-transform: uppercase;
		font-family: monospace;

		color: rgba(255,255,255,0.35);
	}

	.timeline-content h3 {
		font-size: clamp(2.3rem, 3vw, 3.4rem);
	    max-width: 12ch;
		line-height: 1;
		letter-spacing: -0.03em;
		font-family: serif;

		color: white;
	}

	.desc {
		margin-top: 24px;

		font-size: 17px;
		line-height: 1.9;

		color: rgba(255,255,255,0.5);
	}

	.bottom-statement {
		margin-top: 180px;

		text-align: center;
	}

	.statement-line {
		width: 1px;
		height: 120px;

		margin-inline: auto;
		margin-bottom: 40px;

		background:
			linear-gradient(
				to bottom,
				transparent,
				rgba(197,160,89,0.7),
				transparent
			);
	}

	.bottom-statement p {
		font-size: clamp(2rem, 4vw, 4rem);
		line-height: 1.15;
		letter-spacing: -0.04em;
		font-family: serif;

		color: rgba(255,255,255,0.92);
	}

	@media (max-width: 900px) {
	.timeline-row {
		grid-template-columns: 1fr;
		gap: 30px;
	}

	.timeline-row.reverse .timeline-content,
	.timeline-row:not(.reverse) .timeline-content {
		grid-column: auto;
		justify-self: start;
		text-align: left;
	}

	.timeline-number-wrap {
		justify-content: flex-start;
	}

	.content-line {
		display: none;
	}

	.timeline-content {
		width: 100%;
		max-width: 100%;
	}

	.timeline-content h3 {
		max-width: 100%;
		font-size: 2.4rem;
	}
}
</style>