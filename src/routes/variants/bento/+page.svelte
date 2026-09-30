<script lang="ts">
	import { currentRole, projects, openSource, publications, socials } from '$lib/data';

	const featured = projects[0];
	const others = projects.slice(1);

	let time = $state('--:--');
	let seconds = $state('--');
	let dateLabel = $state('Europe/Berlin');
	let hour = $state<number | null>(null);

	$effect(() => {
		const hm = new Intl.DateTimeFormat('en-GB', {
			timeZone: 'Europe/Berlin',
			hour: '2-digit',
			minute: '2-digit',
			second: '2-digit',
			hour12: false
		});
		const day = new Intl.DateTimeFormat('en-GB', {
			timeZone: 'Europe/Berlin',
			weekday: 'long',
			day: 'numeric',
			month: 'short'
		});
		const tick = () => {
			const now = new Date();
			const parts = hm.formatToParts(now);
			const get = (t: string) => parts.find((p) => p.type === t)?.value ?? '';
			time = `${get('hour')}:${get('minute')}`;
			seconds = get('second');
			hour = Number(get('hour'));
			dateLabel = day.format(now);
		};
		tick();
		const id = setInterval(tick, 1000);
		return () => clearInterval(id);
	});

	const daypart = $derived(
		hour === null
			? 'Germany'
			: hour < 6
				? 'Probably asleep'
				: hour < 12
					? 'Morning in Germany'
					: hour < 18
						? 'Afternoon in Germany'
						: 'Evening in Germany'
	);

	function spot(e: PointerEvent) {
		const el = e.currentTarget as HTMLElement;
		const r = el.getBoundingClientRect();
		el.style.setProperty('--mx', `${e.clientX - r.left}px`);
		el.style.setProperty('--my', `${e.clientY - r.top}px`);
	}

	function host(url: string) {
		return url.replace(/^https?:\/\//, '').replace(/\/$/, '');
	}
</script>

<svelte:head>
	<title>Torsten Dittmann — bento</title>
	<meta name="description" content="Torsten Dittmann — Product Architect from Germany." />
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous" />
	<link
		href="https://fonts.googleapis.com/css2?family=Geist:wght@300..700&family=Geist+Mono:wght@400;500&display=swap"
		rel="stylesheet"
	/>
</svelte:head>

<div class="root">
	<div class="aurora" aria-hidden="true">
		<span class="blob b1"></span>
		<span class="blob b2"></span>
		<span class="blob b3"></span>
		<span class="blob b4"></span>
		<span class="blob b5"></span>
		<span class="blob b6"></span>
		<div class="grain"></div>
	</div>

	<main class="relative z-10 mx-auto max-w-6xl px-4 py-6 sm:px-6 sm:py-10 lg:py-14">
		<div class="bento">
			<!-- Intro -->
			<section class="card intro" onpointermove={spot} aria-labelledby="name">
				<div class="flex flex-wrap items-center justify-between gap-2">
					<span class="chip">torsten.work</span>
					<span class="mono hidden text-[11px] tracking-wide text-slate-400 uppercase sm:inline">
						{currentRole.role} · {currentRole.company}
					</span>
				</div>
				<div class="mt-auto">
					<h1 id="name" class="name">Torsten<br />Dittmann</h1>
					<p class="tagline">Product Architect from Germany.</p>
				</div>
			</section>

			<!-- Currently -->
			<section class="card currently" onpointermove={spot} aria-labelledby="now-h">
				<h2 id="now-h" class="label">
					<span class="pulse" aria-hidden="true"></span>
					Currently
				</h2>
				<div class="mt-auto">
					<p class="text-xl leading-tight font-medium text-white">{currentRole.role}</p>
					<p class="mt-1 text-sm text-slate-300">
						at <span class="text-white">{currentRole.company}</span>
					</p>
				</div>
			</section>

			<!-- Clock -->
			<section class="card clock" onpointermove={spot} aria-labelledby="clock-h">
				<h2 id="clock-h" class="label">Local time</h2>
				<div class="mt-auto">
					<p class="time" aria-live="off">
						<span>{time}</span><span class="secs">{seconds}</span>
					</p>
					<p class="mt-1 text-sm text-slate-300">{daypart}</p>
					<p class="mono mt-0.5 text-[11px] text-slate-500">{dateLabel}</p>
				</div>
			</section>

			<!-- Featured -->
			<a
				class="card featured group"
				href={featured.url}
				target="_blank"
				rel="noopener noreferrer"
				onpointermove={spot}
			>
				<div class="flex items-center justify-between gap-3">
					<span class="label">Featured project</span>
					<span class="arrow" aria-hidden="true">↗</span>
				</div>
				<div class="mt-auto">
					<p class="featured-name">{featured.name}</p>
					<p class="mt-2 max-w-md text-[15px] leading-relaxed text-slate-300">
						{featured.description}
					</p>
				</div>
			</a>

			<!-- Open source -->
			<section class="card oss" onpointermove={spot} aria-labelledby="oss-h">
				<h2 id="oss-h" class="label">Open source</h2>
				<ul class="mt-4 flex flex-col">
					{#each openSource as repo (repo.url)}
						<li>
							<a class="row group" href={repo.url} target="_blank" rel="noopener noreferrer">
								<span class="min-w-0">
									<span class="mono block truncate text-[13px] text-white">{repo.name}</span>
									<span class="block text-[13px] leading-snug text-slate-400"
										>{repo.description}</span
									>
								</span>
								<span class="arrow small" aria-hidden="true">↗</span>
							</a>
						</li>
					{/each}
				</ul>
			</section>

			<!-- Other projects -->
			{#each others as p (p.url)}
				<a
					class="card project group"
					class:deprecated={p.deprecated}
					href={p.url}
					target="_blank"
					rel="noopener noreferrer"
					onpointermove={spot}
				>
					<div class="flex items-start justify-between gap-2">
						{#if p.deprecated}
							<span class="chip muted">Deprecated</span>
						{:else}
							<span class="mono text-[11px] tracking-wide text-slate-500 uppercase">Project</span>
						{/if}
						<span class="arrow" aria-hidden="true">↗</span>
					</div>
					<div class="mt-auto pt-6">
						<p class="text-lg font-semibold tracking-tight text-white" class:strike={p.deprecated}>
							{p.name}
						</p>
						<p class="mt-1 text-[13px] leading-snug text-slate-400">{p.description}</p>
					</div>
				</a>
			{/each}

			<!-- Writing -->
			<section class="card writing" onpointermove={spot} aria-labelledby="writing-h">
				<h2 id="writing-h" class="label">Writing</h2>
				<ul class="mt-auto flex flex-col gap-2 pt-6">
					{#each publications as pub (pub.url)}
						<li>
							<a class="pub group" href={pub.url} target="_blank" rel="noopener noreferrer">
								<span class="text-[17px] leading-snug font-medium text-white">{pub.title}</span>
								<span class="mono mt-2 block text-xs text-slate-400">
									{pub.publication} · {pub.year}
								</span>
							</a>
						</li>
					{/each}
				</ul>
			</section>

			<!-- Socials -->
			<section class="card socials" onpointermove={spot} aria-labelledby="social-h">
				<h2 id="social-h" class="label">Elsewhere</h2>
				<ul class="mt-auto grid grid-cols-3 gap-2 pt-6">
					{#each socials as s (s.url)}
						<li>
							<a class="social" href={s.url} target="_blank" rel="noopener noreferrer me">
								<span>{s.name}</span>
								<span class="arrow small" aria-hidden="true">↗</span>
							</a>
						</li>
					{/each}
				</ul>
			</section>
		</div>

		<footer class="mono mt-8 flex justify-between gap-4 px-1 text-[11px] text-slate-500">
			<span>© {new Date().getFullYear()} Torsten Dittmann</span>
			<span>{host('https://torsten.work')}</span>
		</footer>
	</main>
</div>

<style>
	.root {
		--bg: #05060d;
		--card: rgba(255, 255, 255, 0.045);
		--line: rgba(255, 255, 255, 0.09);
		--accent: #8b9cff;
		position: relative;
		min-height: 100vh;
		overflow: hidden;
		isolation: isolate;
		background: var(--bg);
		color: #e5e7f0;
		font-family: 'Geist', 'Inter', system-ui, sans-serif;
		-webkit-font-smoothing: antialiased;
	}
	.mono,
	.label,
	.chip,
	.time {
		font-family: 'Geist Mono', ui-monospace, monospace;
	}

	/* Aurora */
	.aurora {
		position: absolute;
		inset: 0;
		overflow: hidden;
		pointer-events: none;
		z-index: 0;
	}
	.blob {
		position: absolute;
		border-radius: 9999px;
		filter: blur(90px);
		opacity: 0.6;
		will-change: transform;
	}
	.b1 {
		width: 42rem;
		height: 42rem;
		left: -12rem;
		top: -14rem;
		background: radial-gradient(circle, #4f46e5 0%, transparent 65%);
		animation: drift1 26s ease-in-out infinite alternate;
	}
	.b2 {
		width: 36rem;
		height: 36rem;
		right: -10rem;
		top: 8rem;
		background: radial-gradient(circle, #0ea5a4 0%, transparent 65%);
		animation: drift2 32s ease-in-out infinite alternate;
	}
	.b3 {
		width: 40rem;
		height: 40rem;
		left: 20%;
		bottom: -18rem;
		background: radial-gradient(circle, #c026d3 0%, transparent 65%);
		opacity: 0.35;
		animation: drift3 38s ease-in-out infinite alternate;
	}
	.b4 {
		width: 28rem;
		height: 28rem;
		left: 45%;
		top: 30%;
		background: radial-gradient(circle, #2563eb 0%, transparent 65%);
		opacity: 0.3;
		animation: drift1 44s ease-in-out infinite alternate-reverse;
	}
	.b5 {
		width: 34rem;
		height: 34rem;
		right: -8rem;
		top: 55%;
		background: radial-gradient(circle, #7c3aed 0%, transparent 65%);
		opacity: 0.4;
		animation: drift2 36s ease-in-out infinite alternate-reverse;
	}
	.b6 {
		width: 30rem;
		height: 30rem;
		left: -10rem;
		top: 70%;
		background: radial-gradient(circle, #0891b2 0%, transparent 65%);
		opacity: 0.35;
		animation: drift3 40s ease-in-out infinite alternate;
	}
	.grain {
		position: absolute;
		inset: 0;
		background-image:
			radial-gradient(rgba(255, 255, 255, 0.035) 1px, transparent 1px),
			linear-gradient(to bottom, transparent, rgba(5, 6, 13, 0.6));
		background-size:
			3px 3px,
			100% 100%;
	}
	@keyframes drift1 {
		to {
			transform: translate(18vw, 12vh) scale(1.15);
		}
	}
	@keyframes drift2 {
		to {
			transform: translate(-16vw, 18vh) scale(0.9);
		}
	}
	@keyframes drift3 {
		to {
			transform: translate(12vw, -14vh) scale(1.1);
		}
	}

	/* Grid */
	.bento {
		display: grid;
		gap: 0.75rem;
		grid-template-columns: repeat(2, minmax(0, 1fr));
	}
	.bento > :global(*) {
		grid-column: span 2;
	}
	.bento > .currently,
	.bento > .clock {
		grid-column: span 1;
	}
	@media (min-width: 640px) {
		.bento {
			grid-template-columns: repeat(2, minmax(0, 1fr));
			gap: 0.875rem;
		}
		.bento > .project {
			grid-column: span 1;
		}
	}
	@media (min-width: 1024px) {
		.bento {
			grid-template-columns: repeat(4, minmax(0, 1fr));
			grid-auto-rows: minmax(11.5rem, auto);
			grid-auto-flow: dense;
			gap: 1rem;
		}
		.intro,
		.oss {
			grid-row: span 2;
		}
	}

	/* Card */
	.card {
		--mx: 50%;
		--my: -200px;
		position: relative;
		display: flex;
		flex-direction: column;
		min-width: 0;
		padding: 1.375rem;
		border-radius: 1.5rem;
		background:
			linear-gradient(180deg, rgba(255, 255, 255, 0.06), rgba(255, 255, 255, 0.015)), var(--card);
		border: 1px solid var(--line);
		box-shadow:
			inset 0 1px 0 rgba(255, 255, 255, 0.08),
			0 20px 40px -24px rgba(0, 0, 0, 0.8);
		backdrop-filter: blur(22px) saturate(140%);
		-webkit-backdrop-filter: blur(22px) saturate(140%);
		color: inherit;
		text-decoration: none;
		transition:
			transform 0.35s cubic-bezier(0.2, 0.7, 0.2, 1),
			border-color 0.35s,
			box-shadow 0.35s;
	}
	/* spotlight fill */
	.card::before {
		content: '';
		position: absolute;
		inset: 0;
		border-radius: inherit;
		background: radial-gradient(
			420px circle at var(--mx) var(--my),
			rgba(139, 156, 255, 0.1),
			transparent 45%
		);
		opacity: 0;
		transition: opacity 0.4s;
		pointer-events: none;
	}
	/* spotlight border */
	.card::after {
		content: '';
		position: absolute;
		inset: -1px;
		border-radius: inherit;
		padding: 1px;
		background: radial-gradient(
			260px circle at var(--mx) var(--my),
			rgba(170, 185, 255, 0.85),
			transparent 60%
		);
		-webkit-mask:
			linear-gradient(#000 0 0) content-box,
			linear-gradient(#000 0 0);
		-webkit-mask-composite: xor;
		mask-composite: exclude;
		opacity: 0;
		transition: opacity 0.4s;
		pointer-events: none;
	}
	.card:hover::before,
	.card:hover::after,
	.card:focus-within::after {
		opacity: 1;
	}
	a.card:hover,
	a.card:focus-visible {
		transform: translateY(-3px);
		box-shadow:
			inset 0 1px 0 rgba(255, 255, 255, 0.1),
			0 30px 50px -24px rgba(0, 0, 0, 0.9);
	}
	.card > :global(*) {
		position: relative;
		z-index: 1;
	}

	a:focus-visible {
		outline: 2px solid var(--accent);
		outline-offset: 3px;
	}

	.label {
		display: flex;
		align-items: center;
		gap: 0.5rem;
		font-size: 11px;
		font-weight: 500;
		letter-spacing: 0.12em;
		text-transform: uppercase;
		color: #94a3b8;
	}
	.chip {
		display: inline-flex;
		align-items: center;
		gap: 0.4rem;
		padding: 0.3rem 0.65rem;
		border-radius: 9999px;
		font-size: 11px;
		color: #c7d0ff;
		background: rgba(139, 156, 255, 0.1);
		border: 1px solid rgba(139, 156, 255, 0.25);
	}
	.chip.muted {
		color: #fbbf24;
		background: rgba(251, 191, 36, 0.08);
		border-color: rgba(251, 191, 36, 0.28);
		padding: 0.2rem 0.55rem;
		font-size: 10px;
		letter-spacing: 0.06em;
		text-transform: uppercase;
	}

	/* Intro */
	.intro {
		min-height: 20rem;
		padding: 1.75rem;
		background:
			radial-gradient(120% 80% at 0% 0%, rgba(99, 102, 241, 0.22), transparent 60%),
			radial-gradient(80% 60% at 100% 100%, rgba(20, 184, 166, 0.14), transparent 60%),
			linear-gradient(180deg, rgba(255, 255, 255, 0.07), rgba(255, 255, 255, 0.02));
	}
	.name {
		font-size: clamp(3rem, 11vw, 5.25rem);
		line-height: 0.92;
		font-weight: 600;
		letter-spacing: -0.045em;
		background: linear-gradient(180deg, #ffffff 30%, #a5b4fc 120%);
		-webkit-background-clip: text;
		background-clip: text;
		color: transparent;
	}
	.tagline {
		margin-top: 1rem;
		font-size: 1.125rem;
		color: #cbd5e1;
	}

	/* Currently */
	.pulse {
		position: relative;
		width: 8px;
		height: 8px;
		border-radius: 9999px;
		background: #34d399;
		box-shadow: 0 0 12px #34d399;
	}
	.pulse::after {
		content: '';
		position: absolute;
		inset: 0;
		border-radius: inherit;
		background: #34d399;
		animation: ping 1.8s cubic-bezier(0, 0, 0.2, 1) infinite;
	}
	@keyframes ping {
		75%,
		100% {
			transform: scale(2.6);
			opacity: 0;
		}
	}
	.currently,
	.clock {
		min-height: 10.5rem;
	}

	/* Clock */
	.time {
		display: flex;
		align-items: baseline;
		gap: 0.3rem;
		font-size: 2.6rem;
		line-height: 1;
		font-weight: 500;
		letter-spacing: -0.04em;
		color: #fff;
		font-variant-numeric: tabular-nums;
	}
	.secs {
		font-size: 0.95rem;
		color: var(--accent);
		letter-spacing: 0;
	}

	/* Featured */
	.featured {
		min-height: 12rem;
		background:
			radial-gradient(90% 120% at 100% 0%, rgba(192, 38, 211, 0.2), transparent 55%),
			radial-gradient(70% 90% at 0% 100%, rgba(79, 70, 229, 0.18), transparent 60%),
			linear-gradient(180deg, rgba(255, 255, 255, 0.07), rgba(255, 255, 255, 0.02));
	}
	.featured-name {
		font-size: clamp(2rem, 6vw, 2.75rem);
		line-height: 1;
		font-weight: 600;
		letter-spacing: -0.035em;
		color: #fff;
	}

	.arrow {
		display: inline-grid;
		place-items: center;
		width: 2rem;
		height: 2rem;
		flex: none;
		border-radius: 9999px;
		border: 1px solid var(--line);
		color: #cbd5e1;
		font-size: 0.85rem;
		transition:
			transform 0.3s,
			background 0.3s,
			color 0.3s;
	}
	.arrow.small {
		width: 1.5rem;
		height: 1.5rem;
		font-size: 0.7rem;
		border-color: transparent;
		color: #64748b;
	}
	.group:hover .arrow,
	.group:focus-visible .arrow {
		transform: translate(2px, -2px);
		background: #fff;
		color: #05060d;
	}

	/* Projects */
	.project {
		min-height: 11.5rem;
	}
	.project.deprecated {
		background:
			repeating-linear-gradient(135deg, rgba(255, 255, 255, 0.018) 0 8px, transparent 8px 16px),
			var(--card);
	}
	.strike {
		color: #94a3b8;
		text-decoration: line-through;
		text-decoration-color: rgba(251, 191, 36, 0.6);
	}

	/* OSS rows */
	.row {
		display: flex;
		align-items: center;
		justify-content: space-between;
		gap: 1rem;
		padding: 0.6rem 0.65rem;
		margin: 0 -0.65rem;
		border-radius: 0.75rem;
		text-decoration: none;
		border-top: 1px solid rgba(255, 255, 255, 0.05);
		transition: background 0.25s;
	}
	li:first-child > .row {
		border-top-color: transparent;
	}
	.row:hover,
	.row:focus-visible {
		background: rgba(255, 255, 255, 0.05);
	}

	.pub {
		display: block;
		padding: 0.9rem 1rem;
		border-radius: 1rem;
		background: rgba(255, 255, 255, 0.03);
		border: 1px solid rgba(255, 255, 255, 0.06);
		text-decoration: none;
		transition:
			background 0.25s,
			border-color 0.25s;
	}
	.pub:hover {
		background: rgba(255, 255, 255, 0.06);
		border-color: rgba(255, 255, 255, 0.14);
	}

	.social {
		display: flex;
		align-items: center;
		justify-content: space-between;
		gap: 0.25rem;
		padding: 0.8rem 0.6rem 0.8rem 0.9rem;
		border-radius: 0.9rem;
		background: rgba(255, 255, 255, 0.03);
		border: 1px solid rgba(255, 255, 255, 0.07);
		color: #e2e8f0;
		font-size: 0.925rem;
		font-weight: 500;
		text-decoration: none;
		transition:
			background 0.25s,
			border-color 0.25s,
			color 0.25s;
	}
	.social:hover {
		background: rgba(255, 255, 255, 0.08);
		border-color: rgba(255, 255, 255, 0.18);
		color: #fff;
	}
	.social:hover .arrow {
		color: #fff;
	}

	@media (max-width: 639px) {
		.intro {
			min-height: 21rem;
			padding: 1.5rem;
		}
		.card {
			padding: 1.15rem;
			border-radius: 1.25rem;
		}
		.project {
			min-height: 0;
		}
		.time {
			font-size: 2.1rem;
		}
		.currently p:first-child {
			font-size: 1.05rem;
		}
		.blob {
			filter: blur(70px);
		}
	}

	@media (prefers-reduced-motion: reduce) {
		.blob,
		.pulse::after {
			animation: none;
		}
		.card,
		.arrow {
			transition: none;
		}
		a.card:hover,
		a.card:focus-visible {
			transform: none;
		}
	}
</style>
