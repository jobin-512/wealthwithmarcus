<script lang="ts">
	const quickLinks = [
		{ label: 'Home', href: '/' },
		{ label: 'About', href: '/about' },
		{ label: 'Loan Programs', href: '/services' },
		{ label: 'Contact', href: '/contact' }
	];

	const services = [
		'Conventional Loans',
		'VA Loans',
		'FHA Loans',
		'DSCR Financing',
		'Business Funding',
		'Commercial Lending'
	];

	const socials = [
		{
			name: 'Instagram',
			href: '#'
		},
		{
			name: 'LinkedIn',
			href: '#'
		},
		{
			name: 'Facebook',
			href: '#'
		}
	];
</script>

<footer class="relative overflow-hidden border-t border-white/10 bg-[var(--theme-color)]">
	<!-- Ambient -->
	<div class="pointer-events-none absolute inset-0">
		<div
			class="absolute left-[-10%] top-[-20%] h-[500px] w-[500px] rounded-full bg-[#c5a059]/[0.08] blur-[120px]"
		></div>

		<div
			class="absolute bottom-[-30%] right-[-10%] h-[600px] w-[600px] rounded-full bg-[#c5a059]/[0.05] blur-[150px]"
		></div>
	</div>

	<!-- Noise -->
	<div class="noise"></div>

	<!-- Gold Thread -->
	<svg
		class="pointer-events-none absolute inset-0 h-full w-full opacity-[0.08]"
		preserveAspectRatio="none"
	>
		<defs>
			<linearGradient id="footer-line" x1="0%" y1="0%" x2="100%" y2="0%">
				<stop offset="0%" stop-color="#c5a059" stop-opacity="0" />
				<stop offset="50%" stop-color="#c5a059" stop-opacity="1" />
				<stop offset="100%" stop-color="#c5a059" stop-opacity="0" />
			</linearGradient>
		</defs>

		<path
			d="M0,120 C400,40 700,220 960,140 S1400,120 1920,180"
			stroke="url(#footer-line)"
			stroke-width="1"
			fill="none"
		>
			<animate
				attributeName="d"
				dur="16s"
				repeatCount="indefinite"
				values="
          M0,120 C400,40 700,220 960,140 S1400,120 1920,180;
          M0,170 C350,250 800,40 960,110 S1450,100 1920,120;
          M0,120 C400,40 700,220 960,140 S1400,120 1920,180"
			/>
		</path>
	</svg>

	<div class="relative z-10 mx-auto max-w-7xl px-6 pb-10 pt-24">
		<!-- TOP -->
		<div
			class="grid grid-cols-1 gap-16 border-b border-white/10 pb-16 lg:grid-cols-[1.2fr_0.8fr_0.8fr_1fr]"
		>
			<!-- Brand -->
			<div>
				<div
					class="mb-6 inline-flex items-center gap-3 rounded-full border border-white/10 bg-white/[0.03] px-5 py-3 backdrop-blur-xl"
				>
					<div class="h-2 w-2 rounded-full bg-[#c5a059]"></div>

					<span class="font-mono text-[11px] uppercase tracking-[0.28em] text-white/50">
						Wealth With Marcus
					</span>
				</div>

				<h3
					class="max-w-md font-serif text-[clamp(2.5rem,4vw,4rem)] leading-[0.95] tracking-[-0.04em] text-white"
				>
					Building Wealth Through Smart Financing.
				</h3>

				<p class="mt-8 max-w-lg text-base leading-relaxed text-white/50">
					Helping individuals, investors, and businesses access custom lending
					strategies with speed, transparency, and confidence.
				</p>
			</div>

			<!-- Quick Links -->
			<div>
				<p
					class="mb-7 font-mono text-[11px] uppercase tracking-[0.3em] text-white/35"
				>
					Navigation
				</p>

				<div class="space-y-4">
					{#each quickLinks as link}
						<a href={link.href} class="footer-link">
							{link.label}
						</a>
					{/each}
				</div>
			</div>

			<!-- Services -->
			<div>
				<p
					class="mb-7 font-mono text-[11px] uppercase tracking-[0.3em] text-white/35"
				>
					Loan Programs
				</p>

				<div class="space-y-4">
					{#each services as item}
						<p class="footer-link">
							{item}
						</p>
					{/each}
				</div>
			</div>

			<!-- Contact -->
			<div>
				<p
					class="mb-7 font-mono text-[11px] uppercase tracking-[0.3em] text-white/35"
				>
					Contact
				</p>

				<div class="space-y-6">
					<div>
						<p class="footer-label">Email</p>

						<a
							href="mailto:marcus@wealthwithmarcus.com"
							class="footer-contact"
						>
							marcus@wealthwithmarcus.com
						</a>
					</div>

					<div>
						<p class="footer-label">Phone</p>

						<a href="tel:+19497873239" class="footer-contact">
							+1 949-787-3239
						</a>
					</div>

					<div>
						<p class="footer-label">Office</p>

						<p class="footer-contact max-w-xs">
							17911 Von Karman Ave Suite 400, Irvine, CA 92614
						</p>
					</div>
				</div>
			</div>
		</div>

		<!-- BOTTOM -->
		<div
			class="flex flex-col items-center justify-between gap-8 pt-8 lg:flex-row"
		>
			<p class="text-sm text-white/35">
				© 2026 Wealth With Marcus. All rights reserved.
			</p>

			<div class="flex items-center gap-5">
				{#each socials as social}
					<a href={social.href} class="social-pill">
						{social.name}
					</a>
				{/each}
			</div>
		</div>
	</div>
</footer>

<style>
	.noise {
		position: absolute;
		inset: 0;

		opacity: 0.025;

		background-image: radial-gradient(
			rgba(255, 255, 255, 0.35) 0.5px,
			transparent 0.5px
		);

		background-size: 4px 4px;

		pointer-events: none;
	}

	.footer-link {
		display: inline-block;

		color: rgba(255, 255, 255, 0.55);

		font-size: 15px;

		transition:
			transform 0.4s ease,
			color 0.4s ease;
	}

	.footer-link:hover {
		color: white;
		transform: translateX(6px);
	}

	.footer-label {
		margin-bottom: 8px;

		font-size: 10px;
		font-family: monospace;

		text-transform: uppercase;
		letter-spacing: 0.24em;

		color: rgba(255, 255, 255, 0.35);
	}

	.footer-contact {
		color: rgba(255, 255, 255, 0.78);

		font-size: 15px;
		line-height: 1.7;

		transition: color 0.4s ease;
	}

	.footer-contact:hover {
		color: #c5a059;
	}

	.social-pill {
		display: inline-flex;
		align-items: center;
		justify-content: center;

		padding: 12px 18px;

		border-radius: 9999px;

		border: 1px solid rgba(255, 255, 255, 0.08);

		background: rgba(255, 255, 255, 0.03);

		backdrop-filter: blur(14px);

		color: rgba(255, 255, 255, 0.7);

		font-size: 13px;

		transition:
			background 0.4s ease,
			border-color 0.4s ease,
			transform 0.4s ease,
			color 0.4s ease;
	}

	.social-pill:hover {
		transform: translateY(-4px);

		background: rgba(197, 160, 89, 0.08);

		border-color: rgba(197, 160, 89, 0.35);

		color: white;
	}
</style>