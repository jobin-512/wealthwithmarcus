<script lang="ts">
	import { onMount } from 'svelte';

	let sectionRef: HTMLElement;
	let graphRef: HTMLElement;
	let chartLine: SVGPathElement;
	let stats: HTMLElement[] = [];

	const comparisons = [
		{
			old: 'Refinance Again',
			new: 'Restructure Cash Flow',
			desc: 'One restarts debt. The other accelerates principal reduction.',
			value: '30 YEARS → 7 YEARS'
		},
		{
			old: 'Monthly Payment Thinking',
			new: 'Daily Interest Thinking',
			desc: 'Interest accrues daily — but most people only think monthly.',
			value: 'DAILY CONTROL'
		},
		{
			old: 'Equity Locked Away',
			new: 'Equity In Motion',
			desc: 'Unused equity becomes dead capital sitting behind a static loan.',
			value: '$500K+ ACCESS'
		}
	];

	onMount(() => {
		let cleanup: (() => void) | undefined;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			const ctx = gsap.context(() => {
				const pathLength = chartLine.getTotalLength();

				gsap.set(chartLine, {
					strokeDasharray: pathLength,
					strokeDashoffset: pathLength
				});

				gsap.to(chartLine, {
					strokeDashoffset: 0,
					duration: 2.2,
					ease: 'power2.out',
					scrollTrigger: {
						trigger: graphRef,
						start: 'top 75%'
					}
				});

				gsap.from('.headline-line', {
					yPercent: 120,
					opacity: 0,
					stagger: 0.08,
					duration: 1.2,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 78%'
					}
				});

				gsap.from(stats, {
					y: 80,
					opacity: 0,
					stagger: 0.12,
					duration: 1,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: stats[0],
						start: 'top 85%'
					}
				});

				gsap.to('.graph-glow', {
					scale: 1.2,
					opacity: 0.5,
					duration: 4,
					repeat: -1,
					yoyo: true,
					ease: 'sine.inOut'
				});
			}, sectionRef);

			cleanup = () => ctx.revert();
		};

		init();

		return () => cleanup?.();
	});
</script>

<section
	bind:this={sectionRef}
	class="relative overflow-hidden border-t border-white/6 bg-[#030303] py-32 lg:py-44"
>
	<!-- Background -->
	<div class="absolute inset-0">
		<div
			class="absolute left-[-10%] top-[15%] h-[520px] w-[520px] rounded-full bg-[#c5a059]/[0.06] blur-[120px]"
		></div>

		<div
			class="absolute right-[-10%] top-[40%] h-[500px] w-[500px] rounded-full bg-[#c5a059]/[0.05] blur-[130px]"
		></div>
	</div>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<!-- Heading -->
		<div class="mb-24 max-w-5xl">
			<div
				class="mb-7 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl"
			>
				<div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

				<span class="font-mono text-[11px] uppercase tracking-[0.32em] text-white/50">
					Why The Math Changes
				</span>
			</div>

			<h2
				class="overflow-hidden font-serif text-[clamp(3rem,6vw,6.5rem)] leading-[0.95] tracking-[-0.05em] text-white"
			>
				<div class="headline-line">Most People</div>

				<div class="headline-line text-white/35">
					Optimize Payments.
				</div>

				<div class="headline-line italic text-[#c5a059]">
					The Wealthy Optimize Cash Flow.
				</div>
			</h2>

			<p class="mt-10 max-w-3xl text-lg leading-relaxed text-white/55 lg:text-xl">
				The traditional mortgage system is designed around long timelines.
				Strategic lending restructures how money flows through debt — compressing
				interest exposure and accelerating principal reduction.
			</p>
		</div>

		<!-- Main Layout -->
		<div class="grid grid-cols-1 gap-20 lg:grid-cols-[1.1fr_0.9fr] lg:items-center">
			<!-- LEFT GRAPH -->
			<div bind:this={graphRef} class="relative">
				<div
					class="relative overflow-hidden rounded-[38px] border border-white/10 bg-white/[0.03] p-10 backdrop-blur-2xl"
				>
					<!-- Glow -->
					<div
						class="graph-glow absolute left-1/2 top-1/2 h-[340px] w-[340px] -translate-x-1/2 -translate-y-1/2 rounded-full bg-[#c5a059]/10 blur-[100px]"
					></div>

					<!-- Labels -->
					<div class="mb-14 flex items-center justify-between">
						<div>
							<p
								class="mb-2 font-mono text-[10px] uppercase tracking-[0.28em] text-white/35"
							>
								Traditional Mortgage
							</p>

							<p class="text-sm text-white/50">
								Slow amortization over decades
							</p>
						</div>

						<div class="text-right">
							<p
								class="mb-2 font-mono text-[10px] uppercase tracking-[0.28em] text-[#c5a059]"
							>
								Strategic Structure
							</p>

							<p class="text-sm text-white/50">
								Accelerated principal reduction
							</p>
						</div>
					</div>

					<!-- SVG GRAPH -->
					<div class="relative">
						<svg
							viewBox="0 0 800 400"
							class="w-full"
							fill="none"
						>
							<!-- Grid -->
							{#each Array(5) as _, i}
								<line
									x1="0"
									y1={i * 100}
									x2="800"
									y2={i * 100}
									stroke="rgba(255,255,255,0.06)"
								/>
							{/each}

							<!-- OLD PATH -->
							<path
								d="M0 40 C 120 50, 220 80, 320 140 S 600 280, 800 360"
								stroke="rgba(255,255,255,0.22)"
								stroke-width="3"
								stroke-linecap="round"
							/>

							<!-- NEW PATH -->
							<path
								bind:this={chartLine}
								d="M0 40 C 100 60, 180 90, 260 130 S 420 190, 800 210"
								stroke="#c5a059"
								stroke-width="4"
								stroke-linecap="round"
							/>

							<!-- Dots -->
							<circle cx="260" cy="130" r="8" fill="#c5a059" />
							<circle cx="800" cy="210" r="8" fill="#c5a059" />
						</svg>

						<!-- Stats -->
						<div class="mt-10 flex flex-wrap gap-5">
							<div
								class="rounded-full border border-white/10 bg-black/40 px-5 py-3 font-mono text-[10px] uppercase tracking-[0.25em] text-white/40"
							>
								Lower Interest Exposure
							</div>

							<div
								class="rounded-full border border-[#c5a059]/20 bg-[#c5a059]/10 px-5 py-3 font-mono text-[10px] uppercase tracking-[0.25em] text-[#c5a059]"
							>
								Faster Principal Reduction
							</div>
						</div>
					</div>
				</div>
			</div>

			<!-- RIGHT CONTENT -->
			<div class="space-y-14">
				{#each comparisons as item, i}
					<div
						bind:this={stats[i]}
						class="group relative"
					>
						<div class="mb-5 flex items-center gap-4">
							<div
								class="h-px w-14 bg-gradient-to-r from-[#c5a059] to-transparent"
							></div>

							<span
								class="font-mono text-[11px] uppercase tracking-[0.3em] text-white/35"
							>
								Shift {i + 1}
							</span>
						</div>

						<div class="grid grid-cols-[1fr_auto_1fr] gap-5">
							<div>
								<p
									class="mb-2 text-sm uppercase tracking-[0.2em] text-white/30"
								>
									Old Thinking
								</p>

								<h3
									class="text-[1.8rem] leading-none tracking-[-0.03em] text-white/35"
								>
									{item.old}
								</h3>
							</div>

							<div
								class="flex items-center text-[#c5a059]"
							>
								→
							</div>

							<div>
								<p
									class="mb-2 text-sm uppercase tracking-[0.2em] text-[#c5a059]/70"
								>
									New Strategy
								</p>

								<h3
									class="text-[1.8rem] leading-none tracking-[-0.03em] text-white"
								>
									{item.new}
								</h3>
							</div>
						</div>

						<p
							class="mt-6 max-w-xl text-[15px] leading-[1.9] text-white/50 transition-all duration-500 group-hover:text-white/70"
						>
							{item.desc}
						</p>

						<div
							class="mt-7 inline-flex items-center rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 font-mono text-[10px] uppercase tracking-[0.25em] text-[#c5a059]"
						>
							{item.value}
						</div>
					</div>
				{/each}
			</div>
		</div>
	</div>
</section>