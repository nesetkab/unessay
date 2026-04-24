<script lang="ts">
	import '../app.css';
	let { children } = $props();
</script>

<svelte:head>
	<link rel="icon" href="data:image/svg+xml,<svg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'><text y='.9em' font-size='90'>🏛</text></svg>" />
</svelte:head>

<div class="credit-bar">
	<span>made by neset :3 <a href="https://nesetk.com">nesetk.com</a></span>
</div>

<div class="crt-wrapper">
	{@render children()}
	<div class="crt-overlay" aria-hidden="true">
		<div class="crt-scanline"></div>
	</div>
</div>

<style>
	.credit-bar {
		position: fixed;
		top: 0;
		left: 0;
		right: 0;
		z-index: 9999;
		background: var(--bg);
		border-bottom: 1px solid var(--border);
		padding: 0.4rem 1.25rem;
		font-family: var(--mono);
		font-size: 0.625rem;
		letter-spacing: 0.05em;
		color: var(--fg-faint);
		text-align: center;
	}
	.credit-bar a {
		color: var(--fg-dim);
		border-bottom: 1px solid var(--fg-faint);
		transition: color 0.2s;
	}
	.credit-bar a:hover {
		color: var(--accent);
	}

	/* CRT wrapper */
	.crt-wrapper {
		position: relative;
		min-height: 100vh;
		padding-top: 1.75rem;
	}

	/* Full-screen CRT overlay */
	.crt-overlay {
		position: fixed;
		inset: 0;
		z-index: 9000;
		pointer-events: none;
		overflow: hidden;
	}

	/* Static scanlines */
	.crt-overlay::before {
		content: '';
		position: absolute;
		inset: 0;
		background: linear-gradient(
			to bottom,
			rgba(18, 16, 16, 0) 50%,
			rgba(0, 0, 0, 0.15) 50%
		);
		background-size: 100% 4px;
		z-index: 1;
	}

	/* Vignette */
	.crt-overlay::after {
		content: '';
		position: absolute;
		inset: 0;
		background: radial-gradient(
			ellipse at center,
			transparent 60%,
			rgba(0, 0, 0, 0.45) 100%
		);
		z-index: 2;
	}

	/* Sweeping scanline */
	.crt-scanline {
		position: absolute;
		left: 0;
		right: 0;
		height: 80px;
		background: linear-gradient(
			0deg,
			rgba(0, 0, 0, 0) 0%,
			rgba(200, 195, 184, 0.06) 10%,
			rgba(0, 0, 0, 0) 100%
		);
		z-index: 3;
		animation: sweep 8s linear infinite;
	}

	@keyframes sweep {
		0% { top: -80px; }
		80% { top: -80px; }
		100% { top: 100%; }
	}

	@media print {
		.crt-overlay { display: none; }
		.credit-bar { display: none; }
		.crt-wrapper { padding-top: 0; }
	}
</style>
