<script lang="ts">
	import { onMount } from 'svelte';

	let sectionRef: HTMLElement;
	let headlineRef: HTMLElement;
	let graphRef: HTMLElement;
	let statRefs: HTMLElement[] = [];

	const stats = [
		{
			value: '7',
			label: 'Years',
			desc: 'Potential accelerated payoff timeline compared to a traditional 30-year structure.'
		},
		{
			value: 'Daily',
			label: 'Interest Reduction',
			desc: 'Every dollar deposited immediately impacts principal exposure.'
		},
		{
			value: '100%',
			label: 'Cash Flow Control',
			desc: 'Income stays active instead of sitting idle inside low-efficiency accounts.'
		}
	];

	onMount(() => {
		let ctx: any;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			ctx = gsap.context(() => {
				gsap.from(headlineRef, {
					y: 90,
					opacity: 0,
					duration: 1.2,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 75%'
					}
				});

				gsap.from(graphRef, {
					scale: 0.92,
					opacity: 0,
					duration: 1.5,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: graphRef,
						start: 'top 78%'
					}
				});

				gsap.from(statRefs, {
					y: 70,
					opacity: 0,
					stagger: 0.14,
					duration: 1,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: graphRef,
						start: 'top 82%'
					}
				});

				gsap.to('.line-glow', {
					opacity: 1,
					duration: 2,
					repeat: -1,
					yoyo: true,
					ease: 'sine.inOut'
				});
			}, sectionRef);
		};

		init();

		return () => {
			ctx?.revert?.();
		};
	});
</script>

<section
	bind:this={sectionRef}
	class="relative overflow-hidden bg-[var(--theme-color)] py-32 lg:py-44"
>
	<!-- Background -->
	<div class="absolute inset-0">
		<div class="absolute left-[10%] top-[5%] h-[500px] w-[500px] rounded-full bg-[#c5a059]/[0.06] blur-[140px]"></div>

		<div class="absolute bottom-[-10%] right-[0%] h-[700px] w-[700px] rounded-full bg-[#c5a059]/[0.04] blur-[180px]"></div>
	</div>

	<div class="noise"></div>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		
		<!-- HEADER -->
		<div
			bind:this={headlineRef}
			class="mx-auto max-w-4xl text-center"
		>
			<div
				class="mb-8 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl"
			>
				<div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

				<span class="font-mono text-[11px] uppercase tracking-[0.34em] text-white/50">
					Equity Velocity Explained
				</span>
			</div>

			<h2
				class="font-serif text-[clamp(3rem,6vw,6rem)] leading-[0.95] tracking-[-0.04em] text-white"
			>
				Why The
				<span class="italic text-[#c5a059]">First Lien HELOC</span>
				Changes Everything
			</h2>

			<p class="mx-auto mt-10 max-w-3xl text-xl leading-relaxed text-white/55">
				This strategy is not about making larger payments.
			</p>

			<p class="mx-auto mt-4 max-w-3xl text-xl leading-relaxed text-white/55">
				It’s about changing where your money sits
				and how fast principal gets attacked.
			</p>
		</div>

		<!-- VISUAL -->
		<div
			bind:this={graphRef}
			class="relative mt-24 overflow-hidden rounded-[40px] border border-white/10 bg-white/[0.03] p-10 backdrop-blur-2xl lg:p-16"
		>
			<!-- Glow -->
			<div class="absolute inset-0 bg-[radial-gradient(circle_at_top,rgba(197,160,89,0.12),transparent_65%)]"></div>

			<!-- Flow Line -->
			<div class="absolute left-1/2 top-[22%] hidden h-[56%] w-px -translate-x-1/2 bg-gradient-to-b from-transparent via-[#c5a059]/40 to-transparent lg:block"></div>

			<div class="grid grid-cols-1 gap-14 lg:grid-cols-3 lg:gap-10">
				
				<!-- LEFT -->
				<div class="relative">
					<p class="mb-5 font-mono text-[11px] uppercase tracking-[0.26em] text-red-300/60">
						Traditional Flow
					</p>

					<div class="rounded-[28px] border border-red-500/10 bg-red-500/[0.03] p-8">
						<h3 class="font-serif text-4xl leading-none text-white">
							Income →
							<br />
							Checking →
							<br />
							Mortgage
						</h3>

						<p class="mt-8 text-base leading-relaxed text-white/55">
							Most income remains idle until monthly bills are due,
							while interest continues accumulating daily.
						</p>
					</div>

					<div class="line-glow absolute right-[-20px] top-1/2 hidden h-[2px] w-10 bg-[#c5a059] opacity-30 blur-sm lg:block"></div>
				</div>

				<!-- CENTER -->
				<div class="relative flex items-center justify-center">
					<div
						class="relative flex h-[220px] w-[220px] items-center justify-center rounded-full border border-[#c5a059]/20 bg-[#c5a059]/[0.05]"
					>
						<div
							class="absolute inset-[18px] rounded-full border border-[#c5a059]/20"
						></div>

						<div
							class="absolute inset-[40px] rounded-full border border-[#c5a059]/20"
						></div>

						<div class="text-center">
							<p class="font-mono text-[11px] uppercase tracking-[0.26em] text-[#c5a059]">
								New Structure
							</p>

							<h3 class="mt-4 font-serif text-5xl leading-none text-white">
								Cash
								<br />
								Flow
							</h3>
						</div>
					</div>
				</div>

				<!-- RIGHT -->
				<div class="relative">
					<p class="mb-5 font-mono text-[11px] uppercase tracking-[0.26em] text-[#c5a059]">
						First Lien HELOC
					</p>

					<div class="rounded-[28px] border border-[#c5a059]/15 bg-[#c5a059]/[0.05] p-8">
						<h3 class="font-serif text-4xl leading-none text-white">
							Income →
							<br />
							HELOC →
							<br />
							Principal
						</h3>

						<p class="mt-8 text-base leading-relaxed text-white/65">
							Your income immediately offsets debt exposure,
							allowing principal to reduce faster while maintaining liquidity.
						</p>
					</div>

					<div class="line-glow absolute left-[-20px] top-1/2 hidden h-[2px] w-10 bg-[#c5a059] opacity-30 blur-sm lg:block"></div>
				</div>
			</div>

			<!-- STATS -->
			<div class="mt-20 grid grid-cols-1 gap-6 border-t border-white/10 pt-12 lg:grid-cols-3">
				{#each stats as stat, i}
					<div
						bind:this={statRefs[i]}
						class="rounded-[24px] border border-white/10 bg-white/[0.03] p-7"
					>
						<div class="flex items-end gap-3">
							<h3 class="font-serif text-6xl leading-none text-white">
								{stat.value}
							</h3>

							<p class="pb-2 text-sm uppercase tracking-[0.22em] text-[#c5a059]">
								{stat.label}
							</p>
						</div>

						<p class="mt-6 text-sm leading-relaxed text-white/55">
							{stat.desc}
						</p>
					</div>
				{/each}
			</div>
		</div>
	</div>
</section>

<style>
	.noise {
		position: absolute;
		inset: 0;
		opacity: 0.025;
		pointer-events: none;
		background-image:
			radial-gradient(rgba(255,255,255,0.35) 0.5px, transparent 0.5px);
		background-size: 4px 4px;
	}
</style>