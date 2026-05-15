<script lang="ts">
	import { onMount } from 'svelte';
    import Industry from "$lib/assets/helloc/8.webp"

	let sectionRef: HTMLElement;
	let imageRef: HTMLElement;
	let textRef: HTMLElement;

	const misconceptions = [
		{
			label: '“Refinancing is always the smartest option.”'
		},
		{
			label: '“A lower rate automatically means lower cost.”'
		},
		{
			label: '“Equity should stay untouched until retirement.”'
		},
		{
			label: '“The bank’s structure is designed for your benefit.”'
		}
	];

	onMount(() => {
		let ctx: any;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			ctx = gsap.context(() => {
				gsap.from(textRef, {
					opacity: 0,
					x: -70,
					duration: 1.3,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 76%'
					}
				});

				gsap.from(imageRef, {
					opacity: 0,
					scale: 0.88,
					rotate: 2,
					duration: 1.4,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: imageRef,
						start: 'top 80%'
					}
				});

				gsap.from('.myth-item', {
					y: 40,
					opacity: 0,
					stagger: 0.12,
					duration: 0.9,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: '.myths-grid',
						start: 'top 88%'
					}
				});

				gsap.to('.floating-img', {
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
		};

		init();

		return () => {
			ctx?.revert();
		};
	});
</script>

<section
	bind:this={sectionRef}
	class="relative overflow-hidden bg-[#050505] py-32 lg:py-44"
>
	<!-- Background -->
	<div class="noise"></div>

	<div
		class="absolute left-[-10%] top-[15%] h-[500px] w-[500px] rounded-full bg-[#c5a059]/[0.08] blur-[130px]"
	></div>

	<div
		class="absolute bottom-[-20%] right-[-10%] h-[600px] w-[600px] rounded-full bg-white/[0.03] blur-[150px]"
	></div>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<div class="grid items-center gap-20 lg:grid-cols-[1.05fr_0.95fr]">
			<!-- LEFT -->
			<div bind:this={textRef}>
				<div
					class="mb-7 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl"
				>
					<div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

					<span
						class="font-mono text-[11px] uppercase tracking-[0.3em] text-white/55"
					>
						The Industry Problem
					</span>
				</div>

				<h2
					class="max-w-3xl font-serif text-[clamp(3rem,5vw,5.8rem)] leading-[0.95] tracking-[-0.04em] text-white"
				>
					Most Homeowners
					<span class="italic text-[#c5a059]">
						Weren’t Given a Strategy.
					</span>
				</h2>

				<p
					class="mt-10 max-w-2xl text-lg leading-relaxed text-white/58 lg:text-xl"
				>
					They were sold a payment.
				</p>

				<p
					class="mt-6 max-w-2xl text-lg leading-relaxed text-white/50"
				>
					The traditional mortgage system is optimized for long-term interest,
					slow amortization, and predictable banking profits —
					not for accelerating your financial freedom.
				</p>

				<div class="myths-grid mt-14 space-y-4">
					{#each misconceptions as item}
						<div
							class="myth-item flex items-center gap-5 rounded-[22px] border border-white/10 bg-white/[0.03] px-6 py-5 backdrop-blur-xl"
						>
							<div
								class="flex h-10 w-10 shrink-0 items-center justify-center rounded-full border border-[#c5a059]/30 bg-[#c5a059]/10"
							>
								<div
									class="h-2 w-2 rounded-full bg-[#c5a059]"
								></div>
							</div>

							<p
								class="text-base leading-relaxed text-white/72"
							>
								{item.label}
							</p>
						</div>
					{/each}
				</div>

				<div class="mt-14 flex items-center gap-5">
					<div class="h-px flex-1 bg-white/10"></div>

					<p
						class="font-mono text-[10px] uppercase tracking-[0.32em] text-white/35"
					>
						Wealth Strategy Over Loan Sales
					</p>
				</div>
			</div>

			<!-- RIGHT -->
			<div bind:this={imageRef} class="relative">
				<!-- Glow -->
				<div
					class="absolute inset-0 scale-[1.04] rounded-[34px] bg-[#c5a059]/10 blur-[90px]"
				></div>

				<div
					class="relative overflow-hidden rounded-[34px] border border-white/10 bg-white/[0.03]"
				>
					<div
						class="absolute inset-0 z-10 bg-gradient-to-t from-black via-black/20 to-transparent"
					></div>

					<img
						src={Industry}
						alt="Financial strategy planning"
						class="floating-img h-[760px] w-full object-cover"
					/>

					<!-- Floating Insight -->
					<div
						class="absolute bottom-8 left-8 right-8 z-20 rounded-[28px] border border-white/10 bg-black/40 p-7 backdrop-blur-2xl"
					>
						<p
							class="font-mono text-[10px] uppercase tracking-[0.28em] text-[#c5a059]"
						>
							Key Insight
						</p>

						<h3
							class="mt-4 font-serif text-3xl leading-tight text-white"
						>
							A low interest rate means very little
							if the structure keeps you financially trapped.
						</h3>

						<div
							class="mt-8 flex items-center gap-4 border-t border-white/10 pt-6"
						>
							<div
								class="flex h-12 w-12 items-center justify-center rounded-full border border-white/10 bg-white/[0.04]"
							>
								<svg
									class="h-5 w-5 text-[#c5a059]"
									fill="none"
									viewBox="0 0 24 24"
									stroke="currentColor"
									stroke-width="1.7"
								>
									<path
										stroke-linecap="round"
										stroke-linejoin="round"
										d="M13 10V3L4 14h7v7l9-11h-7z"
									/>
								</svg>
							</div>

							<p
								class="max-w-sm text-sm leading-relaxed text-white/55"
							>
								The goal is not just reducing payments.
								It’s increasing control over capital.
							</p>
						</div>
					</div>
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
			radial-gradient(rgba(255,255,255,0.3) 0.5px, transparent 0.5px);
		background-size: 4px 4px;
	}

	img {
		will-change: transform;
	}

	@media (max-width: 768px) {
		img {
			height: 560px !important;
		}
	}
</style>