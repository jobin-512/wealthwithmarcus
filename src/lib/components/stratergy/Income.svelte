<script lang="ts">
	import { onMount } from 'svelte';
    import Luxury from "$lib/assets/stratergy/3.webp"

	let sectionRef: HTMLElement;
	let imageRef: HTMLElement;
	let quoteRef: HTMLElement;
	let textBlocks: HTMLElement[] = [];

	const points = [
		{
			label: 'Traditional Thinking',
			text: 'Most homeowners think the solution is refinancing into another lower rate — even if it resets the debt timeline for another 30 years.'
		},
		{
			label: 'The Hidden Problem',
			text: 'A lower rate does not automatically create wealth. What matters is how efficiently your cash flow attacks principal over time.'
		},
		{
			label: 'The Breakthrough',
			text: 'When income moves through a strategic lending structure, interest exposure drops faster because principal reduces continuously instead of slowly.'
		}
	];

	onMount(() => {
		let cleanup: (() => void) | undefined;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			const ctx = gsap.context(() => {
				gsap.from('.headline-reveal', {
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

				gsap.from(imageRef, {
					scale: 0.9,
					opacity: 0,
					duration: 1.5,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: imageRef,
						start: 'top 82%'
					}
				});

				gsap.from(textBlocks, {
					x: 60,
					opacity: 0,
					stagger: 0.14,
					duration: 1,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: textBlocks[0],
						start: 'top 85%'
					}
				});

				gsap.from(quoteRef, {
					y: 50,
					opacity: 0,
					duration: 1.2,
					delay: 0.2,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: quoteRef,
						start: 'top 90%'
					}
				});

				gsap.to(imageRef, {
					yPercent: -8,
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
	class="relative overflow-hidden bg-[var(--theme-color)] py-32 lg:py-44"
>
	<!-- Background -->
	<div class="absolute inset-0">
		<div
			class="absolute left-[5%] top-[10%] h-[420px] w-[420px] rounded-full bg-[#c5a059]/[0.08] blur-[120px]"
		></div>

		<div
			class="absolute bottom-[0%] right-[0%] h-[520px] w-[520px] rounded-full bg-[#c5a059]/[0.05] blur-[140px]"
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
					The “Wait… What?” Moment
				</span>
			</div>

			<h2
				class="overflow-hidden font-serif text-[clamp(3rem,6vw,6.5rem)] leading-[0.95] tracking-[-0.05em] text-white"
			>
				<div class="headline-reveal">
					The Goal Isn’t
				</div>

				<div class="headline-reveal text-white/35">
					To Borrow Less.
				</div>

				<div class="headline-reveal italic text-[#c5a059]">
					It’s To Use Money Smarter.
				</div>
			</h2>
		</div>

		<!-- Split Layout -->
		<div class="grid grid-cols-1 gap-20 lg:grid-cols-[1fr_0.95fr] lg:items-center">
			<!-- Left Visual -->
			<div class="relative">
				<div
					bind:this={imageRef}
					class="relative overflow-hidden rounded-[36px] border border-white/10"
				>
					<img
						src={Luxury}
						alt="Luxury home financial strategy"
						class="h-[720px] w-full object-cover"
					/>

					<!-- overlays -->
					<div
						class="absolute inset-0 bg-gradient-to-t from-black via-black/20 to-transparent"
					></div>

					<div
						class="absolute inset-0 bg-[radial-gradient(circle_at_top,rgba(197,160,89,0.22),transparent_55%)]"
					></div>

					<!-- floating metric -->
					<div
						class="absolute left-8 top-8 rounded-[26px] border border-white/10 bg-black/40 p-6 backdrop-blur-2xl"
					>
						<p
							class="mb-2 font-mono text-[10px] uppercase tracking-[0.28em] text-[#c5a059]"
						>
							Common Mistake
						</p>

						<h3 class="text-[2.5rem] leading-none tracking-[-0.04em] text-white">
							30
						</h3>

						<p class="mt-2 text-sm text-white/45">
							Years restarted over and over
						</p>
					</div>

					<!-- quote -->
					<div
						bind:this={quoteRef}
						class="absolute bottom-8 left-8 right-8 rounded-[30px] border border-white/10 bg-black/45 p-8 backdrop-blur-2xl"
					>
						<p
							class="mb-4 font-mono text-[10px] uppercase tracking-[0.28em] text-white/35"
						>
							The Realization
						</p>

						<p class="max-w-2xl text-2xl leading-[1.45] text-white">
							“Most people spend decades chasing lower rates…
							while never realizing the structure itself is the problem.”
						</p>
					</div>
				</div>
			</div>

			<!-- Right Content -->
			<div>
				<div class="space-y-16">
					{#each points as point, i}
						<div
							bind:this={textBlocks[i]}
							class="group relative"
						>
							<div class="mb-5 flex items-center gap-5">
								<div
									class="flex h-11 w-11 items-center justify-center rounded-full border border-white/10 bg-white/[0.03] font-mono text-[11px] text-[#c5a059]"
								>
									0{i + 1}
								</div>

								<div class="h-px flex-1 bg-white/10"></div>
							</div>

							<p
								class="mb-4 font-mono text-[11px] uppercase tracking-[0.28em] text-[#c5a059]"
							>
								{point.label}
							</p>

							<p
								class="max-w-xl text-[1.35rem] leading-[1.8] tracking-[-0.02em] text-white/75 transition-all duration-500 group-hover:text-white"
							>
								{point.text}
							</p>
						</div>
					{/each}
				</div>

				<!-- Bottom statement -->
				<div
					class="mt-20 border-t border-white/10 pt-10"
				>
					<p
						class="max-w-2xl text-[1.8rem] leading-[1.5] tracking-[-0.03em] text-white"
					>
						The wealthiest borrowers don’t ask:
						<span class="text-white/35">
							“How do I get the lowest rate?”
						</span>

						<br /><br />

						They ask:
						<span class="italic text-[#c5a059]">
							“How do I make my money move more efficiently?”
						</span>
					</p>
				</div>
			</div>
		</div>
	</div>
</section>