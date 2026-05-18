<script lang="ts">
	import { onMount } from 'svelte';

	let sectionRef: HTMLElement;
	let panelRef: HTMLElement;
	let headlineRef: HTMLElement;
	let buttonsRef: HTMLElement;

	onMount(() => {
		let ctx: any;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			ctx = gsap.context(() => {
				gsap.from(panelRef, {
					scale: 0.92,
					opacity: 0,
					duration: 1.3,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 75%'
					}
				});

				gsap.from(headlineRef.children, {
					y: 90,
					opacity: 0,
					stagger: 0.06,
					duration: 1,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: headlineRef,
						start: 'top 82%'
					}
				});

				gsap.from(buttonsRef.children, {
					stagger: 0.12,
					duration: 0.8,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: buttonsRef,
						start: 'top 90%'
					}
				});

				gsap.to('.pulse-ring', {
					scale: 1.12,
					opacity: 0,
					repeat: -1,
					duration: 2.5,
					ease: 'power2.out'
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
	class="relative overflow-hidden bg-[var(--theme-color)] py-32 lg:py-44"
>
	<!-- Background -->
	<div class="noise"></div>

	<div
		class="absolute left-[-10%] top-[10%] h-[600px] w-[600px] rounded-full bg-[#c5a059]/[0.1] blur-[150px]"
	></div>

	<div
		class="absolute bottom-[-20%] right-[-10%] h-[700px] w-[700px] rounded-full bg-white/[0.03] blur-[180px]"
	></div>

	<!-- Gold Thread -->
	<svg
		class="pointer-events-none absolute inset-0 h-full w-full opacity-[0.08]"
		preserveAspectRatio="none"
	>
		<defs>
			<linearGradient id="gold-end" x1="0%" y1="0%" x2="100%" y2="0%">
				<stop offset="0%" stop-color="#c5a059" stop-opacity="0" />
				<stop offset="50%" stop-color="#c5a059" stop-opacity="1" />
				<stop offset="100%" stop-color="#c5a059" stop-opacity="0" />
			</linearGradient>
		</defs>

		<path
			d="M0,180 C350,80 700,280 960,160 S1450,120 1920,220"
			stroke="url(#gold-end)"
			stroke-width="1"
			fill="none"
		>
			<animate
				attributeName="d"
				dur="18s"
				repeatCount="indefinite"
				values="
        M0,180 C350,80 700,280 960,160 S1450,120 1920,220;
        M0,220 C420,300 780,80 960,120 S1400,180 1920,140;
        M0,180 C350,80 700,280 960,160 S1450,120 1920,220"
			/>
		</path>
	</svg>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<div
			bind:this={panelRef}
			class="relative overflow-hidden rounded-[42px] border border-white/10 bg-white/[0.04] px-8 py-20 backdrop-blur-2xl lg:px-20 lg:py-28"
		>
			<!-- Glow -->
			<div
				class="absolute inset-0 bg-[radial-gradient(circle_at_top,rgba(197,160,89,0.18),transparent_65%)]"
			></div>

			<!-- Shine -->
			<div class="shine"></div>

			<!-- Floating Rings -->
			<div
				class="pulse-ring absolute left-1/2 top-1/2 h-[500px] w-[500px] -translate-x-1/2 -translate-y-1/2 rounded-full border border-[#c5a059]/10"
			></div>

			<div class="relative z-10 text-center">
				<!-- Badge -->
				<div
					class="mb-10 inline-flex items-center gap-3 rounded-full border border-white/10 bg-black/20 px-6 py-3 backdrop-blur-xl"
				>
					<div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

					<span
						class="font-mono text-[11px] uppercase tracking-[0.34em] text-white/55"
					>
						Your Next Financial Move Starts Here
					</span>
				</div>

				<!-- Headline -->
				<h2
					bind:this={headlineRef}
					class="mx-auto max-w-5xl font-serif text-[clamp(3.2rem,6vw,7rem)] leading-[0.92] tracking-[-0.05em] text-white"
				>
					<span class="inline-block">Stop</span>
					<span class="inline-block">Thinking</span>
					<span class="inline-block text-[#c5a059] italic">
						Like
					</span>
					<span class="inline-block text-[#c5a059] italic">
						A Borrower.
					</span>

					<br />

					<span class="inline-block">
						Start Thinking
					</span>

					<span class="inline-block">
						Like A
					</span>

					<span class="inline-block">
						Strategist.
					</span>
				</h2>

				<p
					class="mx-auto mt-10 max-w-3xl text-lg leading-relaxed text-white/58 lg:text-xl"
				>
					Most people spend decades trapped inside financial structures
					they were never taught to question.
				</p>

				<p
					class="mx-auto mt-5 max-w-2xl text-base leading-relaxed text-white/42"
				>
					The right strategy can completely change the speed,
					flexibility, and control of your financial future.
				</p>

				<!-- CTA -->
				<div
					bind:this={buttonsRef}
					class="mt-16 flex flex-col items-center justify-center gap-5 sm:flex-row"
				>
					<a
						href="/book-call"
						class="group relative inline-flex overflow-hidden rounded-full bg-[#c5a059] px-9 py-5 text-sm font-semibold uppercase tracking-[0.2em] text-black transition-all duration-500 hover:scale-[1.03] hover:shadow-[0_0_60px_rgba(197,160,89,0.28)]"
					>
						<span class="relative z-10 flex items-center gap-3">
							Book Strategy Call

							<svg
								class="h-4 w-4 transition-transform duration-500 group-hover:translate-x-1"
								fill="none"
								viewBox="0 0 24 24"
								stroke="currentColor"
								stroke-width="2.4"
							>
								<path
									stroke-linecap="round"
									stroke-linejoin="round"
									d="M17 8l4 4m0 0l-4 4m4-4H3"
								/>
							</svg>
						</span>

						<div class="btn-shine"></div>
					</a>

					<a
						href="/solutions"
						class="inline-flex items-center justify-center rounded-full border border-white/12 bg-white/[0.03] px-9 py-5 text-sm font-medium uppercase tracking-[0.18em] text-white/80 backdrop-blur-xl transition-all duration-500 hover:border-[#c5a059]/30 hover:bg-[#c5a059]/10 hover:text-white"
					>
						Explore Solutions
					</a>
				</div>

				<!-- Bottom -->
				<div
					class="mt-20 flex flex-wrap items-center justify-center gap-5 border-t border-white/10 pt-10 text-center"
				>
					<div
						class="rounded-full border border-white/10 bg-white/[0.03] px-5 py-3"
					>
						<p
							class="font-mono text-[10px] uppercase tracking-[0.28em] text-white/38"
						>
							First Lien HELOC
						</p>
					</div>

					<div
						class="rounded-full border border-white/10 bg-white/[0.03] px-5 py-3"
					>
						<p
							class="font-mono text-[10px] uppercase tracking-[0.28em] text-white/38"
						>
							Digital HELOC
						</p>
					</div>

					<div
						class="rounded-full border border-white/10 bg-white/[0.03] px-5 py-3"
					>
						<p
							class="font-mono text-[10px] uppercase tracking-[0.28em] text-white/38"
						>
							Reverse Strategy
						</p>
					</div>

					<div
						class="rounded-full border border-white/10 bg-white/[0.03] px-5 py-3"
					>
						<p
							class="font-mono text-[10px] uppercase tracking-[0.28em] text-white/38"
						>
							Cash Flow Optimization
						</p>
					</div>
				</div>
			</div>
		</div>
	</div>
</section>

<style>
	:global(body) {
		background: #040404;
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

	.shine {
		position: absolute;
		inset: 0;
		background:
			linear-gradient(
				115deg,
				transparent 40%,
				rgba(255,255,255,0.05) 50%,
				transparent 60%
			);

		transform: translateX(-120%) rotate(12deg);

		animation: shineMove 9s linear infinite;
	}

	.btn-shine {
		position: absolute;
		inset: 0;
		background:
			linear-gradient(
				115deg,
				transparent 35%,
				rgba(255,255,255,0.4) 50%,
				transparent 65%
			);

		transform: translateX(-120%);
		transition: transform 0.9s ease;
	}

	a:hover .btn-shine {
		transform: translateX(120%);
	}

	@keyframes shineMove {
		from {
			transform: translateX(-120%) rotate(12deg);
		}

		to {
			transform: translateX(120%) rotate(12deg);
		}
	}
</style>