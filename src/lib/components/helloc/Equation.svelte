<script lang="ts">
	import { onMount } from 'svelte';

	let sectionRef: HTMLElement;
	let leftRef: HTMLElement;
	let rightRef: HTMLElement;
	let timelineRefs: HTMLElement[] = [];

	const steps = [
		{
			number: '01',
			title: 'Connect Your Income',
			text: 'Your income flows into the HELOC first instead of sitting idle in a checking account earning nothing.'
		},
		{
			number: '02',
			title: 'Reduce Principal Daily',
			text: 'Because HELOC interest is calculated daily, every dollar lowers the balance immediately and reduces interest exposure.'
		},
		{
			number: '03',
			title: 'Use Equity Strategically',
			text: 'Expenses are paid from the line only when needed, keeping your money working instead of trapped.'
		},
		{
			number: '04',
			title: 'Accelerate Payoff',
			text: 'The result is a faster mortgage payoff path without restarting into another 30-year refinance.'
		}
	];

	onMount(() => {
		let ctx: any;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			ctx = gsap.context(() => {
				gsap.from(leftRef, {
					x: -80,
					opacity: 0,
					duration: 1.3,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 72%'
					}
				});

				gsap.from(rightRef, {
					x: 80,
					opacity: 0,
					duration: 1.3,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 72%'
					}
				});

				gsap.from(timelineRefs, {
					y: 80,
					opacity: 0,
					stagger: 0.18,
					duration: 1,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 75%'
					}
				});

				gsap.to('.pulse-ring', {
					scale: 1.2,
					opacity: 0,
					duration: 2.2,
					repeat: -1,
					ease: 'power2.out'
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
	class="relative overflow-hidden bg-[#050505] py-32 lg:py-44"
>
	<!-- Background -->
	<div class="absolute inset-0">
		<div class="absolute left-[-10%] top-[10%] h-[500px] w-[500px] rounded-full bg-[#c5a059]/[0.08] blur-[140px]"></div>

		<div class="absolute bottom-[-15%] right-[-10%] h-[600px] w-[600px] rounded-full bg-[#c5a059]/[0.06] blur-[160px]"></div>
	</div>

	<div class="noise"></div>

	<!-- Grid -->
	<div class="relative z-10 mx-auto grid max-w-7xl grid-cols-1 gap-20 px-6 lg:grid-cols-[0.95fr_1.05fr] lg:items-center">
		
		<!-- LEFT -->
		<div bind:this={leftRef}>
			<div class="mb-8 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl">
				<div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

				<span class="font-mono text-[11px] uppercase tracking-[0.34em] text-white/55">
					The Strategy Behind It
				</span>
			</div>

			<h2
				class="max-w-xl font-serif text-[clamp(3rem,5vw,5.6rem)] leading-[0.95] tracking-[-0.04em] text-white"
			>
				Why This
				<span class="italic text-[#c5a059]">Changes</span>
				The Entire Mortgage Equation
			</h2>

			<p class="mt-8 max-w-xl text-lg leading-relaxed text-white/60 lg:text-xl">
				Most homeowners only focus on interest rates.
				But the real issue is how cash flow moves through the mortgage itself.
			</p>

			<p class="mt-6 max-w-xl text-lg leading-relaxed text-white/60">
				The First Lien HELOC strategy restructures how money flows —
				so your income works against principal every single day.
			</p>

			<!-- Floating Stat -->
			<div class="relative mt-14 max-w-md overflow-hidden rounded-[30px] border border-white/10 bg-white/[0.04] p-8 backdrop-blur-2xl">
				<div class="absolute inset-0 bg-[radial-gradient(circle_at_top,rgba(197,160,89,0.14),transparent_70%)]"></div>

				<div class="relative z-10">
					<p class="font-mono text-[11px] uppercase tracking-[0.3em] text-white/40">
						Traditional Mortgage
					</p>

					<div class="mt-5 flex items-end gap-4">
						<h3 class="font-serif text-6xl leading-none text-white">
							30
						</h3>

						<p class="pb-2 text-white/50">
							Years of scheduled payments
						</p>
					</div>

					<div class="mt-7 h-px bg-white/10"></div>

					<p class="mt-7 text-sm leading-relaxed text-white/50">
						The bank designed the system for maximum interest collection —
						not maximum speed for your payoff.
					</p>
				</div>
			</div>
		</div>

		<!-- RIGHT -->
		<div bind:this={rightRef} class="relative">
			
			<!-- Vertical Line -->
			<div class="absolute left-[26px] top-0 h-full w-px bg-gradient-to-b from-[#c5a059]/0 via-[#c5a059]/40 to-[#c5a059]/0"></div>

			<div class="space-y-14">
				{#each steps as step, i}
					<div
						bind:this={timelineRefs[i]}
						class="relative flex gap-8"
					>
						<!-- Number -->
						<div class="relative z-10 flex-shrink-0">
							<div class="pulse-ring absolute inset-0 rounded-full border border-[#c5a059]/40"></div>

							<div
								class="relative flex h-[54px] w-[54px] items-center justify-center rounded-full border border-[#c5a059]/30 bg-[#0b0b0b] font-mono text-sm text-[#c5a059]"
							>
								{step.number}
							</div>
						</div>

						<!-- Content -->
						<div class="pt-2">
							<h3 class="text-2xl font-semibold tracking-tight text-white">
								{step.title}
							</h3>

							<p class="mt-4 max-w-lg text-base leading-relaxed text-white/55">
								{step.text}
							</p>
						</div>
					</div>
				{/each}
			</div>

			<!-- Bottom Note -->
			<div class="mt-16 rounded-[30px] border border-[#c5a059]/20 bg-[#c5a059]/[0.05] p-8 backdrop-blur-xl">
				<p class="text-lg leading-relaxed text-white/75">
					You are not replacing your mortgage.
				</p>

				<p class="mt-3 font-serif text-3xl leading-[1.2] tracking-tight text-white">
					You are changing how your money interacts with debt.
				</p>
			</div>
		</div>
	</div>
</section>

<style>
	.noise {
		position: absolute;
		inset: 0;
		opacity: 0.03;
		background-image: radial-gradient(rgba(255,255,255,0.35) 0.5px, transparent 0.5px);
		background-size: 4px 4px;
		pointer-events: none;
	}
</style>