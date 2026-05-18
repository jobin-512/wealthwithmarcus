<script lang="ts">
	import { onMount } from 'svelte';

	let sectionRef: HTMLElement;
	let spotlightRef: HTMLElement;
	let formRef: HTMLElement;

	let headlineWords: HTMLElement[] = $state([]);
	let contactRefs: HTMLElement[] = $state([]);

	let formData = {
		firstName: '',
		lastName: '',
		email: '',
		phone: '',
		message: '',
		consent1: false,
		consent2: false
	};

	const contactInfo = [
		{
			icon: 'M3 8l7.89 5.26a2 2 0 002.22 0L21 8M5 19h14a2 2 0 002-2V7a2 2 0 00-2-2H5a2 2 0 00-2 2v10a2 2 0 002 2z',
			label: 'marcus@wealthwithmarcus.com',
			href: 'mailto:marcus@wealthwithmarcus.com',
			title: 'Email Us'
		},
		{
			icon: 'M3 5a2 2 0 012-2h3.28a1 1 0 01.948.684l1.498 4.493a1 1 0 01-.502 1.21l-2.257 1.13a11.042 11.042 0 005.516 5.516l1.13-2.257a1 1 0 011.21-.502l4.493 1.498a1 1 0 01.684.949V19a2 2 0 01-2 2h-1C9.716 21 3 14.284 3 6V5z',
			label: '+1 949-787-3239',
			href: 'tel:+19497873239',
			title: 'Call Anytime'
		},
		{
			icon: 'M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z M15 11a3 3 0 11-6 0 3 3 0 016 0z',
			label: '17911 Von Karman Ave Suite 400, Irvine, CA 92614',
			href: 'https://maps.google.com/?q=17911+Von+Karman+Ave+Suite+400+Irvine+CA+92614',
			title: 'Visit Office'
		}
	];

	onMount(() => {
		let cleanup: (() => void) | undefined;

		const init = async () => {
			const gsap = (await import('gsap')).default;
			const { ScrollTrigger } = await import('gsap/dist/ScrollTrigger');

			gsap.registerPlugin(ScrollTrigger);

			// Spotlight movement
			const xTo = gsap.quickTo(spotlightRef, 'x', {
				duration: 0.8,
				ease: 'power3.out'
			});

			const yTo = gsap.quickTo(spotlightRef, 'y', {
				duration: 0.8,
				ease: 'power3.out'
			});

			const moveSpotlight = (e: MouseEvent) => {
				const rect = sectionRef.getBoundingClientRect();

				xTo(e.clientX - rect.left - 250);
				yTo(e.clientY - rect.top - 250);
			};

			sectionRef.addEventListener('mousemove', moveSpotlight);

			const ctx = gsap.context(() => {
				// HEADLINE
				gsap.from(headlineWords, {
					y: 120,
					opacity: 0,
					rotateX: -90,
					stagger: 0.06,
					duration: 1.3,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: sectionRef,
						start: 'top 80%'
					}
				});

				// LEFT CONTACT CARDS
				gsap.from(contactRefs, {
					opacity: 0,
					stagger: 0.16,
					duration: 1.1,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: contactRefs[0],
						start: 'top 90%'
					}
				});

				// FORM
				gsap.from(formRef, {
					x: 100,
					opacity: 0,
					duration: 1.3,
					ease: 'power4.out',
					scrollTrigger: {
						trigger: formRef,
						start: 'top 88%'
					}
				});

				// FORM FIELDS
				gsap.from('.form-field', {
					y: 40,
					opacity: 0,
					stagger: 0.08,
					duration: 0.8,
					ease: 'power3.out',
					scrollTrigger: {
						trigger: formRef,
						start: 'top 85%'
					}
				});
			}, sectionRef);

			cleanup = () => {
				ctx.revert();
				sectionRef?.removeEventListener('mousemove', moveSpotlight);
				ScrollTrigger.getAll().forEach((t) => t.kill());
			};
		};

		init();

		return () => {
			cleanup?.();
		};
	});
</script>

<section
	bind:this={sectionRef}
	class="relative overflow-hidden bg-[var(--theme-color)] py-32 lg:py-44"
>
	<!-- BACKGROUND -->
	<div class="noise"></div>

	<div class="mesh-1"></div>
	<div class="mesh-2"></div>

	<div class="vignette"></div>

	<div class="particles"></div>

	<!-- SPOTLIGHT -->
	<div
		bind:this={spotlightRef}
		class="pointer-events-none absolute left-0 top-0 h-[500px] w-[500px] rounded-full bg-[#c5a059]/10 blur-[120px]"
	></div>

	<!-- TOP LINE -->
	<div
		class="absolute inset-x-0 top-0 h-px bg-gradient-to-r from-transparent via-white/15 to-transparent"
	></div>

	<!-- SVG FLOW -->
	<svg
		class="pointer-events-none absolute inset-0 h-full w-full opacity-[0.12]"
		preserveAspectRatio="none"
	>
		<defs>
			<linearGradient id="gold-contact" x1="0%" y1="0%" x2="100%" y2="0%">
				<stop offset="0%" stop-color="#c5a059" stop-opacity="0" />
				<stop offset="50%" stop-color="#c5a059" stop-opacity="1" />
				<stop offset="100%" stop-color="#c5a059" stop-opacity="0" />
			</linearGradient>
		</defs>

		<path
			d="M0,150 C400,50 700,250 960,150 S1400,100 1920,200"
			stroke="url(#gold-contact)"
			stroke-width="1"
			fill="none"
		>
			<animate
				attributeName="d"
				dur="18s"
				repeatCount="indefinite"
				values="
        M0,150 C400,50 700,250 960,150 S1400,100 1920,200;
        M0,200 C300,250 800,50 960,100 S1300,150 1920,100;
        M0,150 C400,50 700,250 960,150 S1400,100 1920,200"
			/>
		</path>
	</svg>

	<div class="relative z-10 mx-auto max-w-7xl px-6">
		<!-- HEADER -->
		<div class="mb-24 max-w-3xl">
			<div
				class="mb-7 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl"
			>
				<div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

				<span class="font-mono text-[11px] uppercase tracking-[0.32em] text-white/55">
					Private Funding. Tailored Mortgage Solutions.
				</span>
			</div>

			<h2
				class="font-serif text-[clamp(3rem,6vw,6rem)] leading-[0.95] tracking-[-0.04em] text-white"
				style="perspective:1000px;"
			>
				{#each 'Let’s Build Your Financial Future'.split(' ') as word, i}
					<span bind:this={headlineWords[i]} class="mr-[0.22em] inline-block origin-bottom">
						{word}
					</span>
				{/each}
			</h2>

			<p class="mt-10 max-w-2xl text-lg leading-relaxed text-white/55 lg:text-xl">
				Whether you’re refinancing, purchasing, leveraging equity, or expanding your
				business — we create custom lending strategies built around your goals.
			</p>
		</div>

		<!-- GRID -->
		<div class="grid grid-cols-1 gap-10 lg:grid-cols-[0.9fr_1.1fr] lg:gap-16">
			<!-- LEFT -->
			<div class="space-y-6">
				{#each contactInfo as info, i}
		<a
			bind:this={contactRefs[i]}
			href={info.href}
			target={info.href.startsWith('http') ? '_blank' : undefined}
			rel={info.href.startsWith('http') ? 'noopener noreferrer' : undefined}
			class="contact-card group"
		>
			<div class="card-border"></div>

			<div class="icon-wrap hidden! md:flex!">
				<svg
					class="h-5 w-5"
					fill="none"
					viewBox="0 0 24 24"
					stroke="currentColor"
					stroke-width="1.7"
				>
					<path stroke-linecap="round" stroke-linejoin="round" d={info.icon} />
				</svg>
			</div>

			<div class="content">
				<p class="card-label">
					{info.title}
				</p>

				<p class="card-value">
					{info.label}
				</p>
			</div>

			<div class="hover-glow"></div>
		</a>
	{/each}
			</div>

			<!-- RIGHT -->
			<div
				bind:this={formRef}
				class="relative overflow-hidden rounded-[38px] border border-white/10 bg-white/[0.045] p-8 shadow-[0_0_100px_rgba(0,0,0,0.45)] backdrop-blur-2xl sm:p-10"
				style="transform-style: preserve-3d;"
			>
				<!-- GLOW -->
				<div
					class="pointer-events-none absolute inset-0 rounded-[38px]"
					style="
            background:
            radial-gradient(circle at top,
            rgba(197,160,89,0.16),
            transparent 60%);
          "
				></div>

				<!-- SHINE -->
				<div class="shine pointer-events-none absolute inset-0"></div>

				<!-- CONTENT -->
				<div class="relative z-10">
					<div class="mb-10">
						<p class="mb-4 font-mono text-[11px] uppercase tracking-[0.3em] text-white/40">
							Start Your Consultation
						</p>

						<h3 class="font-serif text-4xl text-white">
							Tell Us About Your Goals
						</h3>
					</div>

					<form class="space-y-8">
						<!-- ROW -->
						<div class="grid grid-cols-1 gap-8 sm:grid-cols-2">
							<div class="form-field input-wrap">
								<input bind:value={formData.firstName} type="text" placeholder=" " />

								<label>First Name*</label>

								<div class="line"></div>
							</div>

							<div class="form-field input-wrap">
								<input bind:value={formData.lastName} type="text" placeholder=" " />

								<label>Last Name*</label>

								<div class="line"></div>
							</div>
						</div>

						<!-- ROW -->
						<div class="grid grid-cols-1 gap-8 sm:grid-cols-2">
							<div class="form-field input-wrap">
								<input bind:value={formData.email} type="email" placeholder=" " />

								<label>Email Address*</label>

								<div class="line"></div>
							</div>

							<div class="form-field input-wrap">
								<input bind:value={formData.phone} type="tel" placeholder=" " />

								<label>Phone Number*</label>

								<div class="line"></div>
							</div>
						</div>

						<!-- TEXTAREA -->
						<div class="form-field input-wrap">
							<textarea
								bind:value={formData.message}
								rows="5"
								placeholder=" "
							></textarea>

							<label>Tell us about your mortgage or funding needs*</label>

							<div class="line"></div>
						</div>

						<!-- CHECKBOXES -->
						<div class="form-field space-y-5 pt-2">
							<label class="check-row">
								<input type="checkbox" bind:checked={formData.consent1} />

								<div class="checkbox">
									<svg
										class="check-icon"
										fill="none"
										viewBox="0 0 24 24"
										stroke="currentColor"
										stroke-width="3"
									>
										<path
											stroke-linecap="round"
											stroke-linejoin="round"
											d="M5 13l4 4L19 7"
										/>
									</svg>
								</div>

								<span>
									I consent to receive transactional updates and appointment reminders.
								</span>
							</label>

							<label class="check-row">
								<input type="checkbox" bind:checked={formData.consent2} />

								<div class="checkbox">
									<svg
										class="check-icon"
										fill="none"
										viewBox="0 0 24 24"
										stroke="currentColor"
										stroke-width="3"
									>
										<path
											stroke-linecap="round"
											stroke-linejoin="round"
											d="M5 13l4 4L19 7"
										/>
									</svg>
								</div>

								<span>
									I consent to receive marketing offers, rate updates, and promotions.
								</span>
							</label>
						</div>

						<!-- BUTTON -->
						<div class="form-field pt-2">
							<button type="button" class="submit-btn">
								<span class="relative z-10 flex items-center gap-3">
									Book Consultation

									<svg
										class="h-4 w-4 transition-transform duration-500 group-hover:translate-x-1"
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
						</div>

						<!-- FOOT -->
						<div
							class="flex flex-wrap items-center justify-center gap-4 pt-2 text-center text-xs text-white/35"
						>
							<a href="/privacy" class="footer-link">Privacy Policy</a>

							<span>•</span>

							<a href="/terms" class="footer-link">Terms & Conditions</a>
						</div>
					</form>
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
		pointer-events: none;
		background-image: radial-gradient(rgba(255, 255, 255, 0.4) 0.5px, transparent 0.5px);
		background-size: 4px 4px;
	}

	.vignette {
		position: absolute;
		inset: 0;
		pointer-events: none;
		background: radial-gradient(circle at center, transparent 20%, rgba(0, 0, 0, 0.8) 100%);
	}

	.mesh-1,
	.mesh-2 {
		position: absolute;
		border-radius: 9999px;
		filter: blur(120px);
	}

	.mesh-1 {
		top: -200px;
		left: -200px;
		width: 700px;
		height: 700px;
		background: rgba(197, 160, 89, 0.12);
	}

	.mesh-2 {
		bottom: -300px;
		right: -300px;
		width: 800px;
		height: 800px;
		background: rgba(197, 160, 89, 0.08);
	}

	.particles::before {
		content: '';
		position: absolute;
		inset: 0;
		background-image: radial-gradient(#c5a059 1px, transparent 1px);
		background-size: 140px 140px;
		opacity: 0.06;
		animation: drift 30s linear infinite;
	}

	@keyframes drift {
		from {
			transform: translateY(0px);
		}
		to {
			transform: translateY(-120px);
		}
	}

	.card-shine {
		position: absolute;
		inset: 0;
		background: linear-gradient(
			115deg,
			transparent 40%,
			rgba(255, 255, 255, 0.04) 50%,
			transparent 60%
		);

		transform: translateX(-120%) rotate(12deg);

		animation: shineMove 9s linear infinite;
	}

	.shine::before {
		content: '';
		position: absolute;
		top: -150%;
		left: -150%;
		width: 300%;
		height: 300%;

		background: linear-gradient(
			115deg,
			transparent 40%,
			rgba(255, 255, 255, 0.05) 50%,
			transparent 60%
		);

		transform: rotate(12deg);

		animation: shineMove 8s linear infinite;
	}

	.input-wrap {
		position: relative;
	}

	.input-wrap input,
	.input-wrap textarea {
		width: 100%;
		background: transparent;
		border: none;
		border-bottom: 1px solid rgba(255, 255, 255, 0.12);
		padding: 14px 0;
		color: white;
		font-size: 14px;
		outline: none;
		transition: all 0.4s ease;
	}

	.input-wrap label {
		position: absolute;
		left: 0;
		top: 14px;
		font-size: 14px;
		color: rgba(255, 255, 255, 0.4);
		pointer-events: none;
		transition: all 0.35s ease;
	}

	.input-wrap input:focus + label,
	.input-wrap input:not(:placeholder-shown) + label,
	.input-wrap textarea:focus + label,
	.input-wrap textarea:not(:placeholder-shown) + label {
		top: -10px;
		font-size: 11px;
		letter-spacing: 0.15em;
		text-transform: uppercase;
		color: #c5a059;
	}

	.line {
		position: absolute;
		left: 50%;
		bottom: 0;
		width: 0%;
		height: 1px;
		background: #c5a059;
		transition: all 0.45s ease;
	}

	.input-wrap input:focus ~ .line,
	.input-wrap textarea:focus ~ .line {
		left: 0;
		width: 100%;
	}

	.check-row {
		display: flex;
		align-items: flex-start;
		gap: 14px;
		cursor: pointer;
	}

	.check-row input {
		display: none;
	}

	.checkbox {
		width: 18px;
		height: 18px;
		border-radius: 6px;
		border: 1px solid rgba(255, 255, 255, 0.15);
		background: rgba(255, 255, 255, 0.03);
		display: flex;
		align-items: center;
		justify-content: center;
		flex-shrink: 0;
		margin-top: 2px;
		transition: all 0.4s ease;
	}

	.check-icon {
		width: 10px;
		height: 10px;
		color: black;
		opacity: 0;
		transform: scale(0.5);
		transition: all 0.3s ease;
	}

	.check-row input:checked + .checkbox {
		background: #c5a059;
		border-color: #c5a059;
	}

	.check-row input:checked + .checkbox .check-icon {
		opacity: 1;
		transform: scale(1);
	}

	.check-row span {
		font-size: 12px;
		line-height: 1.8;
		color: rgba(255, 255, 255, 0.45);
	}

	.submit-btn {
		position: relative;
		width: 100%;
		overflow: hidden;
		border-radius: 9999px;
		background: white;
		padding: 18px 24px;
		color: black;
		font-size: 14px;
		font-weight: 600;
		transition: all 0.5s ease;
	}

	.submit-btn:hover {
		transform: scale(1.02);
		box-shadow: 0 0 60px rgba(255, 255, 255, 0.14);
	}

	.btn-shine {
		position: absolute;
		inset: 0;
		background: linear-gradient(
			115deg,
			transparent 35%,
			rgba(255, 255, 255, 0.5) 50%,
			transparent 65%
		);

		transform: translateX(-120%);
		transition: transform 0.8s ease;
	}

	.submit-btn:hover .btn-shine {
		transform: translateX(120%);
	}

	.footer-link {
		transition: all 0.3s ease;
	}

	.footer-link:hover {
		color: #c5a059;
	}

	@keyframes shineMove {
		from {
			transform: translateX(-40%) rotate(12deg);
		}

		to {
			transform: translateX(40%) rotate(12deg);
		}
	}

    .contact-card {
	position: relative;
	display: flex;
	align-items: flex-start;
	gap: 22px;

	padding: 32px;
	border-radius: 30px;

	overflow: hidden;

	background:
		linear-gradient(
			180deg,
			rgba(255,255,255,0.06),
			rgba(255,255,255,0.025)
		);

	border: 1px solid rgba(255,255,255,0.08);

	backdrop-filter: blur(24px);

	transition:
		transform 0.5s ease,
		border-color 0.5s ease,
		background 0.5s ease;
}

.contact-card:hover {
	transform: translateY(-6px);

	border-color: rgba(197,160,89,0.35);

	background:
		linear-gradient(
			180deg,
			rgba(255,255,255,0.08),
			rgba(255,255,255,0.035)
		);
}

.card-border {
	position: absolute;
	inset: 0;

	border-radius: inherit;

	padding: 1px;

	background:
		linear-gradient(
			135deg,
			rgba(197,160,89,0.3),
			transparent 40%,
			transparent 60%,
			rgba(197,160,89,0.18)
		);

	-webkit-mask:
		linear-gradient(#fff 0 0) content-box,
		linear-gradient(#fff 0 0);

	-webkit-mask-composite: xor;
	mask-composite: exclude;

	pointer-events: none;
}

.icon-wrap {
	position: relative;
	z-index: 2;

	width: 62px;
	height: 62px;

	display: flex;
	align-items: center;
	justify-content: center;

	border-radius: 9999px;

	background:
		radial-gradient(
			circle at top,
			rgba(197,160,89,0.18),
			rgba(255,255,255,0.04)
		);

	border: 1px solid rgba(255,255,255,0.1);

	color: #f3d39a;

	flex-shrink: 0;

	box-shadow:
		0 0 30px rgba(197,160,89,0.08);

	transition: all 0.5s ease;
}

.contact-card:hover .icon-wrap {
	transform: scale(1.08);

	box-shadow:
		0 0 50px rgba(197,160,89,0.18);
}

.content {
	position: relative;
	z-index: 2;
}

.card-label {
	margin-bottom: 10px;

	font-size: 11px;
	font-family: monospace;
	text-transform: uppercase;
	letter-spacing: 0.28em;

	color: rgba(255,255,255,0.55);
}

.card-value {
	font-size: 20px;
	line-height: 1.6;
	font-weight: 400;

	color: rgba(255,255,255,0.96);

	transition: color 0.4s ease;
}

.contact-card:hover .card-value {
	color: white;
}

.hover-glow {
	position: absolute;
	inset: 0;

	background:
		radial-gradient(
			circle at top right,
			rgba(197,160,89,0.14),
			transparent 55%
		);

	opacity: 0;

	transition: opacity 0.5s ease;

	pointer-events: none;
}

.contact-card:hover .hover-glow {
	opacity: 1;
}
</style>