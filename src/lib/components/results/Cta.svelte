<script lang="ts">
	import { onMount } from 'svelte';

	let sectionRef: HTMLElement;
	let headlineRef: HTMLElement;
	let ctaRef: HTMLElement;
	let buttonRefs: HTMLElement[] = [];

	onMount(() => {
		let cleanup: (() => void) | undefined;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			const ctx = gsap.context(() => {
				// HEADLINE
				gsap.from(headlineRef.children, {
					y: 120,
					opacity: 0,
					rotateX: -80,
					stagger: 0.06,
					duration: 1.2,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: headlineRef,
						start: 'top 82%'
					}
				});

				// CTA BOX
				gsap.from(ctaRef, {
					y: 100,
					opacity: 0,
					scale: 0.96,
					duration: 1.4,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: ctaRef,
						start: 'top 85%'
					}
				});

				// BUTTONS
				gsap.from(buttonRefs, {
					y: 40,
					opacity: 0,
					stagger: 0.12,
					duration: 1,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: ctaRef,
						start: 'top 80%'
					}
				});

				// MAGNETIC BUTTONS
				buttonRefs.forEach((btn) => {
					btn.addEventListener('mousemove', (e: MouseEvent) => {
						const rect = btn.getBoundingClientRect();

						const x = e.clientX - rect.left - rect.width / 2;
						const y = e.clientY - rect.top - rect.height / 2;

						gsap.to(btn, {
							x: x * 0.18,
							y: y * 0.18,
							duration: 0.5,
							ease: 'power3.out'
						});
					});

					btn.addEventListener('mouseleave', () => {
						gsap.to(btn, {
							x: 0,
							y: 0,
							duration: 1,
							ease: 'elastic.out(1,0.45)'
						});
					});
				});

				// FLOATING ORBS
				gsap.to('.orb-float-1', {
					y: -50,
					x: 20,
					duration: 8,
					repeat: -1,
					yoyo: true,
					ease: 'sine.inOut'
				});

				gsap.to('.orb-float-2', {
					y: 40,
					x: -30,
					duration: 10,
					repeat: -1,
					yoyo: true,
					ease: 'sine.inOut'
				});
			}, sectionRef);

			cleanup = () => {
				ctx.revert();
			};
		};

		init();

		return () => cleanup?.();
	});
</script>

<section
	bind:this={sectionRef}
	class="relative overflow-hidden bg-[#050505] py-32 lg:py-44"
>
	<!-- BACKGROUND -->
	<div class="noise"></div>

	<div class="orb orb-float-1"></div>
	<div class="orb orb-float-2"></div>

	<div class="vignette"></div>

	<!-- GRID -->
	<div class="grid-lines"></div>

	<!-- TOP LINE -->
	<div
		class="absolute inset-x-0 top-0 h-px bg-gradient-to-r from-transparent via-white/12 to-transparent"
	></div>

	<!-- SVG -->
	<svg
		class="pointer-events-none absolute inset-0 h-full w-full opacity-[0.08]"
		preserveAspectRatio="none"
	>
		<defs>
			<linearGradient id="results-final-line" x1="0%" y1="0%" x2="100%" y2="0%">
				<stop offset="0%" stop-color="#c5a059" stop-opacity="0" />
				<stop offset="50%" stop-color="#c5a059" stop-opacity="1" />
				<stop offset="100%" stop-color="#c5a059" stop-opacity="0" />
			</linearGradient>
		</defs>

		<path
			d="M0,180 C420,80 740,260 960,180 S1400,100 1920,200"
			stroke="url(#results-final-line)"
			stroke-width="1"
			fill="none"
		>
			<animate
				attributeName="d"
				dur="18s"
				repeatCount="indefinite"
				values="
        M0,180 C420,80 740,260 960,180 S1400,100 1920,200;
        M0,220 C320,260 820,40 960,100 S1450,160 1920,120;
        M0,180 C420,80 740,260 960,180 S1400,100 1920,200"
			/>
		</path>
	</svg>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<!-- HEADING -->
		<div class="mx-auto max-w-5xl text-center">
			<p
				class="mb-7 font-mono text-[11px] uppercase tracking-[0.38em] text-[#c5a059]"
			>
				This Isn’t Traditional Lending
			</p>

			<h2
				bind:this={headlineRef}
				class="font-serif text-[clamp(3rem,6vw,6.5rem)] leading-[0.92] tracking-[-0.05em] text-white"
				style="perspective:1000px;"
			>
				<span class="inline-block">You</span>
				<span class="inline-block">Don’t</span>
				<span class="inline-block text-white/35">Need</span>

				<br />

				<span class="inline-block text-[#c5a059]">Another</span>
				<span class="inline-block">Loan.</span>

				<br />

				<span class="inline-block">You</span>
				<span class="inline-block">Need</span>
				<span class="inline-block">A</span>
				<span class="inline-block text-[#c5a059]">Strategy.</span>
			</h2>

			<p
				class="mx-auto mt-10 max-w-3xl text-lg leading-relaxed text-white/55 lg:text-xl"
			>
				Most financial products are sold in isolation.
				We engineer structures that improve cash flow,
				reduce long-term interest, and create flexibility
				without destroying your existing position.
			</p>
		</div>

		<!-- CTA PANEL -->
		<div
			bind:this={ctaRef}
			class="relative mt-24 overflow-hidden rounded-[40px] border border-white/10 bg-white/[0.03] p-10 backdrop-blur-2xl lg:p-16"
		>
			<!-- GLOW -->
			<div class="panel-glow"></div>

			<!-- SHINE -->
			<div class="panel-shine"></div>

			<div
				class="relative z-10 flex flex-col gap-14 lg:flex-row lg:items-end lg:justify-between"
			>
				<!-- LEFT -->
				<div class="max-w-3xl">
					<p
						class="mb-5 font-mono text-[11px] uppercase tracking-[0.35em] text-white/40"
					>
						Book Your Private Strategy Session
					</p>

					<h3
						class="font-serif text-[clamp(2.3rem,4vw,4.8rem)] leading-[1] tracking-[-0.04em] text-white"
					>
						Access Smarter
						<span class="text-white/35">
							Capital Structures.
						</span>
					</h3>

					<p
						class="mt-8 max-w-2xl text-base leading-relaxed text-white/55 lg:text-lg"
					>
						Whether you’re optimizing a mortgage,
						unlocking dormant equity, or accelerating
						wealth-building strategies — the next move
						should be intentional, not reactive.
					</p>
				</div>

				<!-- BUTTONS -->
				<div class="flex flex-col gap-5 sm:flex-row">
                <a href="/contact" >
					<button
						bind:this={buttonRefs[0]}
						class="cta-primary"
					>
						<span class="relative z-10 flex items-center gap-3">
							Book Strategy Call

							<svg
								class="h-4 w-4"
								fill="none"
								viewBox="0 0 24 24"
								stroke="currentColor"
								stroke-width="2.5"
							>
								<path
									stroke-linecap="round"
									stroke-linejoin="round"
									d="M17 8l4 4m0 0l-4 4m4-4H3"
								/>
							</svg>
						</span>

						<div class="btn-shine"></div>
					</button>
                    </a>

                    <a href="/solutions" >
					<button
						bind:this={buttonRefs[1]}
						class="cta-secondary"
					>
						Explore Solutions
					</button>
                </a>
				</div>
			</div>

			<!-- STATS -->
			<div
				class="relative z-10 mt-14 grid grid-cols-1 gap-8 border-t border-white/10 pt-10 sm:grid-cols-3"
			>
				<div>
					<p class="stat-label">Equity Access</p>
					<h4 class="stat-value">$500K–$4M+</h4>
				</div>

				<div>
					<p class="stat-label">Payoff Strategies</p>
					<h4 class="stat-value">5–7 Years</h4>
				</div>

				<div>
					<p class="stat-label">Built Around</p>
					<h4 class="stat-value">Cash Flow Control</h4>
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

		background-image:
			radial-gradient(rgba(255,255,255,0.3) 0.5px, transparent 0.5px);

		background-size: 4px 4px;

		pointer-events: none;
	}

	.vignette {
		position: absolute;
		inset: 0;

		background:
			radial-gradient(circle at center,
			transparent 18%,
			rgba(0,0,0,0.82) 100%);
	}

	.grid-lines {
		position: absolute;
		inset: 0;

		background-image:
			linear-gradient(rgba(255,255,255,0.025) 1px, transparent 1px),
			linear-gradient(90deg, rgba(255,255,255,0.025) 1px, transparent 1px);

		background-size: 120px 120px;

		mask-image:
			radial-gradient(circle at center,
			black 20%,
			transparent 90%);
	}

	.orb {
		position: absolute;
		border-radius: 9999px;
		filter: blur(140px);
	}

	.orb-float-1 {
		top: -200px;
		left: -200px;

		width: 700px;
		height: 700px;

		background: rgba(197,160,89,0.12);
	}

	.orb-float-2 {
		right: -260px;
		bottom: -260px;

		width: 800px;
		height: 800px;

		background: rgba(197,160,89,0.08);
	}

	.panel-glow {
		position: absolute;
		inset: 0;

		background:
			radial-gradient(circle at top,
			rgba(197,160,89,0.16),
			transparent 60%);
	}

	.panel-shine {
		position: absolute;
		inset: 0;

		background:
			linear-gradient(
				115deg,
				transparent 35%,
				rgba(255,255,255,0.05) 50%,
				transparent 65%
			);

		transform: translateX(-120%) rotate(12deg);

		animation: shineMove 10s linear infinite;
	}

	.cta-primary,
	.cta-secondary {
		position: relative;
		overflow: hidden;

		height: 64px;
		padding: 0 34px;

		border-radius: 9999px;

		font-size: 14px;
		font-weight: 600;

		transition:
			transform 0.4s ease,
			background 0.4s ease,
			border-color 0.4s ease,
			box-shadow 0.4s ease;
	}

	.cta-primary {
		background: #c5a059;
		color: black;

		box-shadow:
			0 0 60px rgba(197,160,89,0.16);
	}

	.cta-primary:hover {
		transform: scale(1.03);

		box-shadow:
			0 0 80px rgba(197,160,89,0.28);
	}

	.cta-secondary {
		border: 1px solid rgba(255,255,255,0.1);

		background: rgba(255,255,255,0.04);

		color: white;

		backdrop-filter: blur(20px);
	}

	.cta-secondary:hover {
		border-color: rgba(197,160,89,0.35);

		background: rgba(255,255,255,0.06);
	}

	.btn-shine {
		position: absolute;
		inset: 0;

		background:
			linear-gradient(
				115deg,
				transparent 35%,
				rgba(255,255,255,0.55) 50%,
				transparent 65%
			);

		transform: translateX(-120%);
		transition: transform 0.8s ease;
	}

	.cta-primary:hover .btn-shine {
		transform: translateX(120%);
	}

	.stat-label {
		margin-bottom: 10px;

		font-size: 10px;
		letter-spacing: 0.3em;
		text-transform: uppercase;

		color: rgba(255,255,255,0.35);
	}

	.stat-value {
		font-family: serif;
		font-size: 34px;
		line-height: 1.1;

		color: white;
	}

	@keyframes shineMove {
		from {
			transform: translateX(-120%) rotate(12deg);
		}

		to {
			transform: translateX(120%) rotate(12deg);
		}
	}

	@media (max-width: 1024px) {
		.stat-value {
			font-size: 28px;
		}
	}

	@media (max-width: 640px) {
		.cta-primary,
		.cta-secondary {
			width: 100%;
		}

		.stat-value {
			font-size: 24px;
		}
	}
</style>