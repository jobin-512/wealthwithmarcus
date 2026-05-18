<script lang="ts">
	import { onMount } from 'svelte';

	let sectionRef: HTMLElement;
	let leftRef: HTMLElement;
	let rightRef: HTMLElement;
	let quoteRef: HTMLElement;

	const scenarios = [
		{
			title: 'Traditional Mortgage',
			years: '30 Years',
			desc: 'Fixed monthly payments with slow principal reduction.'
		},
		{
			title: 'Refinance',
			years: '20–30 More Years',
			desc: 'Lower payment temporarily, but often extends debt longer.'
		},
		{
			title: 'First Lien HELOC',
			years: '5–7 Years',
			desc: 'Accelerated payoff through strategic cash flow movement.'
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
					x: -90,
					opacity: 0,
					duration: 1.2,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 72%'
					}
				});

				gsap.from(rightRef, {
					x: 90,
					opacity: 0,
					duration: 1.2,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 72%'
					}
				});

				gsap.from('.scenario-card', {
					y: 80,
					opacity: 0,
					stagger: 0.15,
					duration: 1,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: rightRef,
						start: 'top 80%'
					}
				});

				gsap.from(quoteRef, {
					scale: 0.95,
					opacity: 0,
					duration: 1.3,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: quoteRef,
						start: 'top 82%'
					}
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
		<div class="absolute left-[5%] top-[10%] h-[600px] w-[600px] rounded-full bg-[#c5a059]/[0.07] blur-[150px]"></div>

		<div class="absolute bottom-[-15%] right-[-10%] h-[700px] w-[700px] rounded-full bg-[#c5a059]/[0.05] blur-[180px]"></div>
	</div>

	<div class="noise"></div>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<div class="grid grid-cols-1 gap-20 lg:grid-cols-[0.85fr_1.15fr] lg:items-start">
			
			<!-- LEFT -->
			<div bind:this={leftRef} class="lg:sticky lg:top-28">
				<div
					class="mb-8 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl"
				>
					<div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

					<span class="font-mono text-[11px] uppercase tracking-[0.34em] text-white/50">
						The Timeline Difference
					</span>
				</div>

				<h2
					class="max-w-md font-serif text-[clamp(3rem,5vw,5.6rem)] leading-[0.95] tracking-[-0.04em] text-white"
				>
					Same House.
					<br />
					<span class="italic text-[#c5a059]">Completely</span>
					Different Future.
				</h2>

				<p class="mt-10 max-w-md text-lg leading-relaxed text-white/58">
					The structure behind your mortgage determines
					whether your income builds wealth —
					or keeps feeding interest for decades.
				</p>

				<!-- Quote -->
				<div
					bind:this={quoteRef}
					class="relative mt-16 overflow-hidden rounded-[32px] border border-[#c5a059]/15 bg-[#c5a059]/[0.05] p-8 backdrop-blur-2xl"
				>
					<div class="absolute inset-0 bg-[radial-gradient(circle_at_top,rgba(197,160,89,0.14),transparent_70%)]"></div>

					<div class="relative z-10">
						<p class="font-serif text-4xl leading-[1.15] tracking-tight text-white">
							“The goal isn’t lower payments.
							<br />
							The goal is faster freedom.”
						</p>

						<div class="mt-8 h-px bg-white/10"></div>

						<p class="mt-6 text-sm uppercase tracking-[0.24em] text-white/40">
							Wealth With Marcus
						</p>
					</div>
				</div>
			</div>

			<!-- RIGHT -->
			<div bind:this={rightRef}>
				<div class="space-y-8">
					{#each scenarios as item, i}
						<div
							class="scenario-card relative overflow-hidden rounded-[36px] border border-white/10 bg-white/[0.03] p-8 backdrop-blur-2xl lg:p-10"
						>
							<!-- Glow -->
							<div
								class="absolute inset-0 bg-[radial-gradient(circle_at_top,rgba(197,160,89,0.10),transparent_70%)]"
							></div>

							<div class="relative z-10 grid grid-cols-1 gap-10 lg:grid-cols-[0.9fr_1.1fr] lg:items-center">
								
								<!-- LEFT -->
								<div>
									<p class="font-mono text-[11px] uppercase tracking-[0.28em] text-[#c5a059]">
										0{i + 1}
									</p>

									<h3
										class="mt-5 font-serif text-5xl leading-[0.95] tracking-[-0.03em] text-white"
									>
										{item.years}
									</h3>

									<div class="mt-8 h-px w-full bg-gradient-to-r from-[#c5a059]/30 to-transparent"></div>

									<p class="mt-7 text-sm uppercase tracking-[0.24em] text-white/35">
										Estimated Debt Timeline
									</p>
								</div>

								<!-- RIGHT -->
								<div>
									<p class="font-mono text-[11px] uppercase tracking-[0.28em] text-white/35">
										{item.title}
									</p>

									<p class="mt-6 text-xl leading-relaxed text-white/75">
										{item.desc}
									</p>

									<div class="mt-10 flex flex-wrap gap-3">
										<div
											class="rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 text-sm text-white/55"
										>
											Cash Flow
										</div>

										<div
											class="rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 text-sm text-white/55"
										>
											Interest Structure
										</div>

										<div
											class="rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 text-sm text-white/55"
										>
											Equity Strategy
										</div>
									</div>
								</div>
							</div>

							<!-- Progress -->
							<div class="relative z-10 mt-10">
								<div class="h-[7px] overflow-hidden rounded-full bg-white/5">
									<div
										class="h-full rounded-full bg-gradient-to-r from-[#c5a059] to-[#f3d6a0]"
										style={`width: ${
											i === 0 ? '18%' : i === 1 ? '28%' : '78%'
										}`}
									></div>
								</div>
							</div>
						</div>
					{/each}
				</div>

				<!-- Bottom Statement -->
				<div class="mt-14 border-l border-[#c5a059]/40 pl-8">
					<p class="max-w-3xl font-serif text-4xl leading-[1.25] tracking-tight text-white">
						Most people spend decades paying interest
						because nobody ever showed them
						how to restructure the flow of money itself.
					</p>
				</div>
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