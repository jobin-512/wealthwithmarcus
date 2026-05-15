<script lang="ts">
	import { onMount } from 'svelte';
    import Interest from "$lib/assets/stratergy/4.webp"

	let sectionRef: HTMLElement;
	let timelineRef: HTMLElement;
	let imageRef: HTMLElement;
	let steps: HTMLElement[] = [];

	const flow = [
		{
			title: 'Income Enters The HELOC',
			desc:
				'Instead of letting income sit idle in a checking account, cash flow immediately reduces principal balance.'
		},
		{
			title: 'Interest Is Calculated Daily',
			desc:
				'Because the balance declines faster, interest exposure continuously shrinks instead of compounding against you.'
		},
		{
			title: 'Expenses Flow Back Out',
			desc:
				'You still maintain liquidity and flexibility while strategically compressing debt over time.'
		},
		{
			title: 'Principal Accelerates Downward',
			desc:
				'The structure creates momentum — reducing years of interest drag without forcing a refinance reset.'
		}
	];

	onMount(() => {
		let cleanup: (() => void) | undefined;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			const ctx = gsap.context(() => {
				gsap.from('.headline > div', {
					yPercent: 120,
					opacity: 0,
					stagger: 0.08,
					duration: 1.15,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 80%'
					}
				});

				gsap.from(imageRef, {
					scale: 0.9,
					opacity: 0,
					duration: 1.4,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: imageRef,
						start: 'top 85%'
					}
				});

				gsap.from(steps, {
					x: 70,
					opacity: 0,
					stagger: 0.14,
					duration: 1,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: timelineRef,
						start: 'top 82%'
					}
				});

				gsap.to(imageRef, {
					yPercent: -10,
					ease: 'none',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top bottom',
						end: 'bottom top',
						scrub: true
					}
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
			class="absolute left-[10%] top-[15%] h-[500px] w-[500px] rounded-full bg-[#c5a059]/[0.07] blur-[140px]"
		></div>

		<div
			class="absolute bottom-[-10%] right-[-5%] h-[620px] w-[620px] rounded-full bg-[#c5a059]/[0.05] blur-[160px]"
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
					Introducing First Lien Strategy
				</span>
			</div>

			<h2
				class="headline overflow-hidden font-serif text-[clamp(3rem,6vw,6.5rem)] leading-[0.95] tracking-[-0.05em] text-white"
			>
				<div>
					This Is Where
				</div>

				<div class="text-white/35">
					The Financial
				</div>

				<div class="italic text-[#c5a059]">
					Math Changes.
				</div>
			</h2>

			<p class="mt-10 max-w-3xl text-lg leading-relaxed text-white/55 lg:text-xl">
				First Lien HELOC strategy isn’t about “another loan.”
				It’s about restructuring how your income interacts with debt —
				so your money works continuously instead of sitting idle.
			</p>
		</div>

		<!-- Layout -->
		<div class="grid grid-cols-1 gap-20 lg:grid-cols-[0.9fr_1.1fr] lg:items-center">
			<!-- LEFT -->
			<div class="relative">
				<div
					bind:this={imageRef}
					class="relative overflow-hidden rounded-[38px] border border-white/10"
				>
					<img
						src={Interest}
						alt="Financial planning strategy"
						class="h-[760px] w-full object-cover"
					/>

					<div
						class="absolute inset-0 bg-gradient-to-t from-black via-black/20 to-transparent"
					></div>

					<div
						class="absolute inset-0 bg-[radial-gradient(circle_at_top,rgba(197,160,89,0.22),transparent_60%)]"
					></div>

					<!-- Floating card -->
					<div
						class="absolute left-8 top-8 rounded-[28px] border border-white/10 bg-black/45 p-7 backdrop-blur-2xl"
					>
						<p
							class="mb-3 font-mono text-[10px] uppercase tracking-[0.28em] text-[#c5a059]"
						>
							Traditional System
						</p>

						<h3 class="text-[2.8rem] leading-none tracking-[-0.04em] text-white">
							Idle
						</h3>

						<p class="mt-3 text-sm leading-relaxed text-white/45">
							Cash sits inactive while interest compounds daily.
						</p>
					</div>

					<!-- Bottom statement -->
					<div
						class="absolute bottom-8 left-8 right-8 rounded-[30px] border border-white/10 bg-black/45 p-8 backdrop-blur-2xl"
					>
						<p
							class="mb-5 font-mono text-[10px] uppercase tracking-[0.28em] text-white/35"
						>
							The Shift
						</p>

						<p class="text-[1.8rem] leading-[1.5] tracking-[-0.03em] text-white">
							“Your income shouldn’t just pay bills.
							It should actively reduce debt every single day.”
						</p>
					</div>
				</div>
			</div>

			<!-- RIGHT -->
			<div bind:this={timelineRef}>
				<div class="relative">
					<!-- timeline line -->
					<div
						class="absolute left-[18px] top-0 h-full w-px bg-gradient-to-b from-[#c5a059] via-white/10 to-transparent"
					></div>

					<div class="space-y-16">
						{#each flow as item, i}
							<div
								bind:this={steps[i]}
								class="group relative pl-16"
							>
								<!-- node -->
								<div
									class="absolute left-0 top-1 flex h-9 w-9 items-center justify-center rounded-full border border-[#c5a059]/30 bg-[#0b0b0b]"
								>
									<div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>
								</div>

								<p
									class="mb-4 font-mono text-[11px] uppercase tracking-[0.28em] text-[#c5a059]"
								>
									Step 0{i + 1}
								</p>

								<h3
									class="mb-5 text-[2.2rem] leading-none tracking-[-0.03em] text-white transition-all duration-500 group-hover:translate-x-2"
								>
									{item.title}
								</h3>

								<p
									class="max-w-xl text-[15px] leading-[1.9] text-white/50 transition-all duration-500 group-hover:text-white/70"
								>
									{item.desc}
								</p>
							</div>
						{/each}
					</div>
				</div>

				<!-- Footer statement -->
				<div
					class="mt-24 border-t border-white/10 pt-10"
				>
					<p
						class="max-w-2xl text-[1.9rem] leading-[1.5] tracking-[-0.03em] text-white"
					>
						The traditional system asks:
						<span class="text-white/35">
							“How long will it take to pay this off?”
						</span>

						<br /><br />

						This strategy asks:
						<span class="italic text-[#c5a059]">
							“How efficiently can your money attack principal?”
						</span>
					</p>
				</div>
			</div>
		</div>
	</div>
</section>