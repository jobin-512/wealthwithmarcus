<script lang="ts">
	import { onMount } from 'svelte';

	let sectionRef: HTMLElement;
	let stickyRef: HTMLElement;

	let rows: HTMLElement[] = [];
	let visualRef: HTMLElement;

	const systems = [
		{
			title: 'Traditional Mortgage',
			desc: 'Fixed payment structure designed to maximize long-term interest collection.',
			stat: '30 Years',
			accent: false
		},
		{
			title: 'Refinance',
			desc: 'Lower monthly payment — but often resets the amortization cycle.',
			stat: 'Restart',
			accent: false
		},
		{
			title: 'First Lien HELOC',
			desc: 'Income attacks principal daily while preserving liquidity and flexibility.',
			stat: 'Velocity',
			accent: true
		}
	];

	onMount(() => {
		let cleanup: (() => void) | undefined;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			const ctx = gsap.context(() => {
				gsap.from(rows, {
					y: 80,
					opacity: 0,
					stagger: 0.14,
					duration: 1.1,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: rows[0],
						start: 'top 85%'
					}
				});

				gsap.from('.money-flow', {
					scaleX: 0,
					transformOrigin: 'left center',
					stagger: 0.15,
					duration: 1.4,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: visualRef,
						start: 'top 82%'
					}
				});

				gsap.to('.pulse-orb', {
					scale: 1.15,
					opacity: 0.8,
					duration: 3,
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
	class="relative overflow-hidden bg-[var(--theme-color)] py-32 lg:py-44"
>
	<!-- ambient -->
	<div class="noise"></div>

	<div
		class="absolute left-[-10%] top-[10%] h-[700px] w-[700px] rounded-full bg-[#c5a059]/[0.07] blur-[150px]"
	></div>

	<div
		class="absolute bottom-[-15%] right-[-10%] h-[800px] w-[800px] rounded-full bg-[#c5a059]/[0.05] blur-[160px]"
	></div>

	<div class="vignette"></div>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<div class="grid gap-20 lg:grid-cols-[0.9fr_1.1fr] lg:gap-28">
			<!-- LEFT -->
			<div bind:this={stickyRef} class="lg:sticky lg:top-32 lg:h-fit">
				<div
					class="mb-7 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl"
				>
					<div class="pulse-orb h-2 w-2 rounded-full bg-[#c5a059]"></div>

					<span class="font-mono text-[11px] uppercase tracking-[0.32em] text-white/50">
						How The Strategy Actually Works
					</span>
				</div>

				<h2
					class="max-w-3xl font-serif text-[clamp(3rem,6vw,6rem)] leading-[0.92] tracking-[-0.05em] text-white"
				>
					Most
					<span class="italic text-[#c5a059]">mortgages</span>

					<br />

					are designed
					<br />

					to move
					<span class="italic text-[#c5a059]">slowly.</span>
				</h2>

				<p class="mt-10 max-w-xl text-lg leading-relaxed text-white/55">
					The difference isn’t just the interest rate.
					It’s how cash flow interacts with the balance every single day.
				</p>

				<div class="mt-14 space-y-8">
					<div>
						<p class="mb-2 text-[11px] uppercase tracking-[0.24em] text-white/30">
							Traditional Flow
						</p>

						<div class="money-flow flow-red"></div>
					</div>

					<div>
						<p class="mb-2 text-[11px] uppercase tracking-[0.24em] text-white/30">
							Strategic Flow
						</p>

						<div class="money-flow flow-gold"></div>
					</div>
				</div>
			</div>

			<!-- RIGHT -->
			<div bind:this={visualRef}>
				<div class="space-y-16">
					{#each systems as item, i}
						<div
							bind:this={rows[i]}
							class="system-row"
						>
							<div class="top">
								<div class="index">
									0{i + 1}
								</div>

								<div class="content">
									<h3 class:gold={item.accent}>
										{item.title}
									</h3>

									<p>
										{item.desc}
									</p>
								</div>

								<div class="stat" class:gold={item.accent}>
									{item.stat}
								</div>
							</div>

							<div class="line-wrap">
								<div class="line"></div>

								<div
									class="active-line"
									style={`width:${i === 0 ? '100%' : i === 1 ? '82%' : '32%'}`}
								></div>
							</div>
						</div>
					{/each}
				</div>

				<!-- quote -->
				<div class="quote-wrap">
					<p>
						“The goal isn’t just a lower payment.
						The goal is to reduce how long your money stays trapped.”
					</p>
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
		opacity: 0.03;
		background-image: radial-gradient(rgba(255,255,255,0.35) 0.5px, transparent 0.5px);
		background-size: 4px 4px;
		pointer-events: none;
	}

	.vignette {
		position: absolute;
		inset: 0;
		pointer-events: none;

		background:
			radial-gradient(circle at center,
			transparent 20%,
			rgba(0,0,0,0.84) 100%);
	}

	.money-flow {
		position: relative;
		width: 100%;
		height: 2px;
		border-radius: 999px;
		overflow: hidden;
		background: rgba(255,255,255,0.08);
	}

	.money-flow::after {
		content: '';
		position: absolute;
		left: 0;
		top: 0;
		height: 100%;
		border-radius: inherit;
	}

	.flow-red::after {
		width: 100%;
		background: linear-gradient(90deg, #ffffff, rgba(255,255,255,0.18));
	}

	.flow-gold::after {
		width: 38%;
		background: linear-gradient(90deg, #c5a059, rgba(255,255,255,0.7));
	}

	.system-row {
		padding-bottom: 36px;
		border-bottom: 1px solid rgba(255,255,255,0.08);
	}

	.top {
		display: grid;
		grid-template-columns: auto 1fr auto;
		gap: 28px;
		align-items: start;
	}

	.index {
		font-size: 11px;
		letter-spacing: 0.28em;
		text-transform: uppercase;
		color: rgba(255,255,255,0.28);
		font-family: monospace;
		padding-top: 8px;
	}

	.content h3 {
		font-size: clamp(2rem, 3vw, 3.2rem);
		line-height: 1;
		letter-spacing: -0.04em;
		color: white;
		font-family: serif;
		margin-bottom: 14px;
	}

	.content h3.gold {
		color: #c5a059;
	}

	.content p {
		max-width: 42ch;
		font-size: 16px;
		line-height: 1.8;
		color: rgba(255,255,255,0.52);
	}

	.stat {
		font-size: clamp(1.5rem, 2vw, 2.6rem);
		line-height: 1;
		letter-spacing: -0.04em;
		color: rgba(255,255,255,0.85);
		font-family: serif;
		white-space: nowrap;
	}

	.stat.gold {
		color: #c5a059;
	}

	.line-wrap {
		position: relative;
		margin-top: 28px;
		height: 2px;
		background: rgba(255,255,255,0.08);
		border-radius: 999px;
		overflow: hidden;
	}

	.active-line {
		position: absolute;
		left: 0;
		top: 0;
		height: 100%;

		background:
			linear-gradient(
				90deg,
				#c5a059,
				rgba(255,255,255,0.8)
			);

		border-radius: inherit;
	}

	.quote-wrap {
		margin-top: 90px;
		padding-top: 50px;
		border-top: 1px solid rgba(255,255,255,0.08);
	}

	.quote-wrap p {
		max-width: 18ch;
		font-size: clamp(2rem, 4vw, 4.5rem);
		line-height: 0.98;
		letter-spacing: -0.05em;
		color: white;
		font-family: serif;
	}

	@media (max-width: 768px) {
		.top {
			grid-template-columns: 1fr;
			gap: 18px;
		}

		.stat {
			font-size: 2rem;
		}

		.quote-wrap p {
			max-width: 100%;
		}
	}
</style>