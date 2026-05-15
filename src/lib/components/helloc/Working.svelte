<script lang="ts">
	import { onMount } from 'svelte';

	let sectionRef: HTMLElement;
	let stickyRef: HTMLElement;
	let timelineRefs: HTMLElement[] = [];

	const phases = [
		{
			step: '01',
			title: 'Analyze Current Structure',
			desc:
				'We identify where your cash flow is leaking, where equity is trapped, and why your current mortgage structure is slowing wealth creation.',
			points: [
				'Mortgage analysis',
				'Equity positioning',
				'Cash-flow mapping',
				'Debt velocity review'
			]
		},
		{
			step: '02',
			title: 'Engineer a Smarter Flow',
			desc:
				'Instead of replacing low-rate debt unnecessarily, we restructure how money moves through your accounts to improve efficiency.',
			points: [
				'First lien HELOC strategy',
				'Liquidity optimization',
				'Daily interest reduction',
				'Capital accessibility'
			]
		},
		{
			step: '03',
			title: 'Deploy the Strategy',
			desc:
				'Your system gets implemented with a tailored roadmap designed around income, goals, timelines, and risk tolerance.',
			points: [
				'Funding coordination',
				'Implementation guidance',
				'Payment flow setup',
				'Strategic oversight'
			]
		},
		{
			step: '04',
			title: 'Accelerate Long-Term Wealth',
			desc:
				'The objective is not just debt reduction. It’s financial control, improved leverage, and long-term optionality.',
			points: [
				'Equity preservation',
				'Cash-flow freedom',
				'Portfolio flexibility',
				'Long-term scaling'
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
				gsap.from('.timeline-card', {
					opacity: 0,
					y: 90,
					stagger: 0.18,
					duration: 1.2,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 72%'
					}
				});

				gsap.from('.line-grow', {
					scaleY: 0,
					transformOrigin: 'top',
					duration: 1.8,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 70%'
					}
				});

				timelineRefs.forEach((card) => {
					gsap.to(card, {
						y: -16,
						ease: 'none',
						scrollTrigger: {
							trigger: card,
							start: 'top bottom',
							end: 'bottom top',
							scrub: true
						}
					});
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
	class="relative overflow-hidden bg-[#070707] py-32 lg:py-44"
>
	<!-- Background -->
	<div class="noise"></div>

	<div
		class="absolute left-[-10%] top-[10%] h-[500px] w-[500px] rounded-full bg-[#c5a059]/[0.08] blur-[140px]"
	></div>

	<div
		class="absolute bottom-[-20%] right-[-10%] h-[600px] w-[600px] rounded-full bg-white/[0.03] blur-[160px]"
	></div>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<div class="grid gap-20 lg:grid-cols-[0.8fr_1.2fr]">
			<!-- LEFT -->
			<div
				bind:this={stickyRef}
				class="lg:sticky lg:top-32 lg:h-fit"
			>
				<div
					class="mb-7 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl"
				>
					<div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

					<span
						class="font-mono text-[11px] uppercase tracking-[0.3em] text-white/55"
					>
						How The System Works
					</span>
				</div>

				<h2
					class="font-serif text-[clamp(3rem,5vw,5.5rem)] leading-[0.95] tracking-[-0.04em] text-white"
				>
					A Strategy Process —
					<span class="italic text-[#c5a059]">
						Not a Loan Process.
					</span>
				</h2>

				<p
					class="mt-8 max-w-lg text-lg leading-relaxed text-white/55"
				>
					Every recommendation starts with one question:
					“How do we create more financial flexibility without destroying existing advantages?”
				</p>

				<div class="mt-14 flex items-center gap-5">
					<div class="h-px flex-1 bg-white/10"></div>

					<p
						class="font-mono text-[10px] uppercase tracking-[0.32em] text-white/35"
					>
						4-Phase Wealth Framework
					</p>
				</div>
			</div>

			<!-- RIGHT -->
			<div class="relative">
				<!-- Vertical line -->
				<div
					class="line-grow absolute left-[28px] top-0 hidden h-full w-px bg-gradient-to-b from-[#c5a059] via-white/15 to-transparent md:block"
				></div>

				<div class="space-y-14">
					{#each phases as phase, i}
						<div
							bind:this={timelineRefs[i]}
							class="timeline-card relative md:pl-20"
						>
							<!-- Dot -->
							<div
								class="absolute left-0 top-0 hidden h-14 w-14 items-center justify-center rounded-full border border-[#c5a059]/30 bg-[#0c0c0c] backdrop-blur-xl md:flex"
							>
								<span
									class="font-serif text-lg text-[#c5a059]"
								>
									{phase.step}
								</span>
							</div>

							<div
								class="relative overflow-hidden rounded-[34px] border border-white/10 bg-white/[0.03] p-8 backdrop-blur-2xl lg:p-10"
							>
								<!-- Glow -->
								<div
									class="absolute inset-0 bg-[radial-gradient(circle_at_top,rgba(197,160,89,0.12),transparent_70%)]"
								></div>

								<div class="relative z-10">
									<div
										class="mb-5 inline-flex items-center gap-3 rounded-full border border-white/10 bg-black/20 px-4 py-2"
									>
										<div
											class="h-2 w-2 rounded-full bg-[#c5a059]"
										></div>

										<span
											class="font-mono text-[10px] uppercase tracking-[0.28em] text-white/45"
										>
											Phase {phase.step}
										</span>
									</div>

									<h3
										class="max-w-xl font-serif text-3xl leading-tight text-white lg:text-4xl"
									>
										{phase.title}
									</h3>

									<p
										class="mt-6 max-w-2xl text-base leading-relaxed text-white/55 lg:text-lg"
									>
										{phase.desc}
									</p>

									<div
										class="mt-10 grid gap-4 sm:grid-cols-2"
									>
										{#each phase.points as point}
											<div
												class="flex items-center gap-3 rounded-2xl border border-white/8 bg-black/20 px-5 py-4"
											>
												<div
													class="h-2 w-2 rounded-full bg-[#c5a059]"
												></div>

												<p
													class="text-sm text-white/70"
												>
													{point}
												</p>
											</div>
										{/each}
									</div>
								</div>
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
		background: #070707;
	}

	.noise {
		position: absolute;
		inset: 0;
		opacity: 0.025;
		pointer-events: none;
		background-image:
			radial-gradient(rgba(255,255,255,0.28) 0.5px, transparent 0.5px);
		background-size: 4px 4px;
	}

	.timeline-card {
		will-change: transform;
	}

	@media (max-width: 1024px) {
		.timeline-card {
			padding-left: 0;
		}
	}
</style>