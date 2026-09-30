<script lang="ts">
	import { currentRole, projects, openSource, publications, socials } from '$lib/data';

	const pad = (n: number) => String(n).padStart(2, '0');
	const hasDeprecated = projects.some((p) => p.deprecated);
</script>

<svelte:head>
	<title>Torsten Dittmann — editorial</title>
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous" />
	<link
		href="https://fonts.googleapis.com/css2?family=Inter+Tight:ital,wght@0,400;0,500;0,600;0,800;0,900;1,400&family=Instrument+Serif:ital@0;1&family=JetBrains+Mono:wght@400;500&display=swap"
		rel="stylesheet"
	/>
</svelte:head>

<div class="page min-h-screen overflow-x-clip">
	<div class="relative mx-auto max-w-[1280px] px-4 sm:px-8">
		<!-- visible column grid -->
		<div
			class="pointer-events-none absolute inset-y-0 right-4 left-4 grid grid-cols-4 gap-x-4 sm:right-8 sm:left-8 md:grid-cols-12 md:gap-x-6"
			aria-hidden="true"
		>
			{#each Array.from({ length: 12 }, (_, i) => i) as i (i)}
				<div class="gridcol {i >= 4 ? 'hidden md:block' : ''}"></div>
			{/each}
		</div>

		<div class="relative">
			<!-- Masthead -->
			<header class="mono pt-5 text-[11px] tracking-[0.08em] uppercase sm:text-xs">
				<div class="rule-double"></div>
				<div class="flex items-center justify-between gap-4 py-2.5">
					<p>
						Issue No. 2026 <span class="text-accent">—</span>
						<span class="hidden sm:inline">Portfolio of Torsten Dittmann</span>
						<span class="sm:hidden">Portfolio</span>
					</p>
					<p class="hidden md:block">Est. Germany · Printed on the Web</p>
					<p>Vol. 01</p>
				</div>
				<div class="rule"></div>
			</header>

			<!-- Hero -->
			<section class="pt-10 pb-12 md:pt-14 md:pb-20" aria-labelledby="name">
				<h1 id="name" class="display name">
					<span class="block">Torsten</span>
					<span class="block md:pl-[16.66%]">Dittmann<span class="text-accent">.</span></span>
				</h1>

				<div class="mt-10 grid grid-cols-4 gap-x-4 gap-y-10 md:mt-16 md:grid-cols-12 md:gap-x-6">
					<!-- Colophon / socials -->
					<aside class="col-span-4 md:col-span-3">
						<p class="label">00 — Contact</p>
						<div class="rule mt-2"></div>
						<ul class="mt-1">
							{#each socials as s (s.url)}
								<li class="border-b border-[var(--ink)]/15">
									<a
										href={s.url}
										class="sweep group flex items-baseline justify-between py-2.5 text-[15px] font-medium"
										target="_blank"
										rel="noopener noreferrer"
									>
										<span class="sweep-text">{s.name}</span>
										<span
											class="mono text-xs transition-transform group-hover:translate-x-0.5 group-hover:-translate-y-0.5 motion-reduce:transition-none"
											aria-hidden="true">↗</span
										>
									</a>
								</li>
							{/each}
						</ul>
					</aside>

					<!-- Pull quote -->
					<div class="col-span-4 md:col-span-8 md:col-start-5">
						<p class="label">Tagline</p>
						<div class="rule mt-2"></div>
						<blockquote class="serif quote mt-5">
							<span class="text-accent">“</span>Product Architect <em>from</em> Germany.<span
								class="text-accent">”</span
							>
						</blockquote>
						<div
							class="mt-8 grid grid-cols-2 gap-x-6 border-t border-[var(--ink)] pt-3 sm:grid-cols-3"
						>
							<div>
								<p class="label">Currently</p>
								<p class="mt-1 text-lg font-semibold tracking-tight">{currentRole.role}</p>
							</div>
							<div>
								<p class="label">At</p>
								<p class="mt-1 text-lg font-semibold tracking-tight">
									{currentRole.company}<sup class="text-accent mono ml-0.5 text-[10px]">1</sup>
								</p>
							</div>
							<div class="hidden sm:block">
								<p class="label">Contents</p>
								<p class="mono mt-1.5 text-xs leading-relaxed">
									01 Projects<br />02 Open Source<br />03 Writing
								</p>
							</div>
						</div>
					</div>
				</div>
			</section>

			<!-- 01 Projects -->
			<section class="pb-16 md:pb-24" aria-labelledby="projects">
				<div class="section-head">
					<span class="num text-accent">01</span>
					<h2 id="projects" class="display section-title">Projects</h2>
					<span class="label ml-auto hidden self-end pb-2 sm:block">Index of works</span>
				</div>
				<ol class="border-t-2 border-[var(--ink)]">
					{#each projects as p, i (p.url)}
						<li class="border-b border-[var(--ink)]">
							<a
								href={p.url}
								target="_blank"
								rel="noopener noreferrer"
								class="row group grid grid-cols-[3.25rem_1fr_auto] items-baseline gap-x-3 px-1 py-5 md:grid-cols-12 md:gap-x-6 md:px-0 md:py-7"
							>
								<span class="display row-num md:col-span-2">{pad(i + 1)}</span>
								<span class="md:col-span-4">
									<span
										class="display block text-[1.6rem] leading-none font-extrabold tracking-[-0.03em] md:text-[2.4rem] {p.deprecated
											? 'line-through decoration-[var(--accent)] decoration-2'
											: ''}">{p.name}</span
									>
									{#if p.deprecated}
										<span class="mono mt-2 inline-block text-[10px] tracking-[0.12em] uppercase">
											<span class="text-accent row-accent">*</span> Discontinued
										</span>
									{/if}
								</span>
								<span
									class="col-span-2 col-start-2 mt-2 text-[15px] leading-snug opacity-80 md:col-span-5 md:col-start-auto md:mt-0 md:text-base"
									>{p.description}</span
								>
								<span
									class="mono col-start-3 row-start-1 self-start pt-1 text-lg transition-transform group-hover:translate-x-1 motion-reduce:transition-none md:col-span-1 md:col-start-12 md:row-start-auto md:self-baseline md:pt-0 md:text-right"
									aria-hidden="true">→</span
								>
							</a>
						</li>
					{/each}
				</ol>
			</section>

			<!-- 02 Open Source -->
			<section class="pb-16 md:pb-24" aria-labelledby="oss">
				<div class="section-head">
					<span class="num text-accent">02</span>
					<h2 id="oss" class="display section-title">Open Source</h2>
					<span class="label ml-auto hidden self-end pb-2 sm:block">Selected repositories</span>
				</div>
				<div
					class="grid grid-cols-4 gap-x-4 border-t-2 border-[var(--ink)] md:grid-cols-12 md:gap-x-6"
				>
					<p
						class="serif col-span-4 pt-5 text-2xl leading-tight italic md:col-span-3 md:text-[1.7rem]"
					>
						Things built in the open, for anyone to use.
					</p>
					<ul class="col-span-4 grid md:col-span-9 md:grid-cols-2 md:gap-x-6">
						{#each openSource as o, i (o.url)}
							<li class="border-b border-[var(--ink)]/25">
								<a
									href={o.url}
									target="_blank"
									rel="noopener noreferrer"
									class="sweep group flex gap-4 py-5"
								>
									<span class="mono text-accent w-8 shrink-0 pt-1 text-xs">2.{i + 1}</span>
									<span class="min-w-0">
										<span class="block text-lg font-semibold tracking-tight break-words">
											<span class="sweep-text">{o.name}</span>
										</span>
										<span class="mt-1 block text-[15px] leading-snug opacity-75"
											>{o.description}</span
										>
									</span>
								</a>
							</li>
						{/each}
					</ul>
				</div>
			</section>

			<!-- 03 Writing -->
			<section class="pb-16 md:pb-24" aria-labelledby="writing">
				<div class="section-head">
					<span class="num text-accent">03</span>
					<h2 id="writing" class="display section-title">Writing</h2>
					<span class="label ml-auto hidden self-end pb-2 sm:block">Published elsewhere</span>
				</div>
				<ul class="border-t-2 border-[var(--ink)]">
					{#each publications as pub (pub.url)}
						<li class="border-b border-[var(--ink)]">
							<a
								href={pub.url}
								target="_blank"
								rel="noopener noreferrer"
								class="sweep group grid grid-cols-4 gap-x-4 gap-y-3 py-7 md:grid-cols-12 md:gap-x-6 md:py-10"
							>
								<span class="col-span-4 md:col-span-3">
									<span class="label block">{pub.publication}</span>
									<span class="display mt-1 block text-5xl font-black tracking-[-0.04em]"
										>{pub.year}</span
									>
								</span>
								<span
									class="serif col-span-4 text-[2rem] leading-[1.05] md:col-span-8 md:col-start-5 md:text-[3.25rem]"
								>
									<span class="sweep-text">{pub.title}</span>
									<span class="text-accent display text-2xl not-italic" aria-hidden="true">↗</span>
								</span>
							</a>
						</li>
					{/each}
				</ul>
			</section>

			<!-- Footnotes -->
			<footer class="pb-10">
				<div class="rule-double"></div>
				<div
					class="mono grid grid-cols-4 gap-x-4 gap-y-3 pt-4 text-[11px] leading-relaxed md:grid-cols-12 md:gap-x-6"
				>
					<ol class="col-span-4 space-y-1 md:col-span-6">
						<li>
							<sup class="text-accent">1</sup> Current position: {currentRole.role} at {currentRole.company}.
						</li>
						{#if hasDeprecated}
							<li>
								<span class="text-accent">*</span> Discontinued project, kept in the index for the record.
							</li>
						{/if}
					</ol>
					<p class="col-span-4 md:col-span-3 md:col-start-8">
						Set in Inter Tight &amp; Instrument Serif on a twelve-column grid.
					</p>
					<p class="col-span-4 md:col-span-2 md:col-start-11 md:text-right">
						© {new Date().getFullYear()} Torsten Dittmann
					</p>
				</div>
			</footer>
		</div>
	</div>
</div>

<style>
	.page {
		--paper: #f2efe8;
		--ink: #121212;
		--accent: #e3200f;
		background: var(--paper);
		color: var(--ink);
		font-family: 'Inter Tight', 'Helvetica Neue', Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
	}
	.page ::selection {
		background: var(--accent);
		color: var(--paper);
	}
	.page a {
		color: inherit;
		text-decoration: none;
	}
	.page a:focus-visible {
		outline: 2px solid var(--accent);
		outline-offset: 3px;
	}
	.text-accent {
		color: var(--accent);
	}
	.display {
		font-family: 'Inter Tight', 'Helvetica Neue', Helvetica, Arial, sans-serif;
	}
	.serif {
		font-family: 'Instrument Serif', Georgia, serif;
		font-weight: 400;
	}
	.mono {
		font-family: 'JetBrains Mono', ui-monospace, monospace;
	}
	.gridcol {
		border-left: 1px solid color-mix(in srgb, var(--accent) 12%, transparent);
		border-right: 1px solid color-mix(in srgb, var(--accent) 12%, transparent);
	}
	.rule {
		height: 1px;
		background: var(--ink);
	}
	.rule-double {
		height: 4px;
		border-top: 2px solid var(--ink);
		border-bottom: 1px solid var(--ink);
	}
	.label {
		font-family: 'JetBrains Mono', ui-monospace, monospace;
		font-size: 11px;
		letter-spacing: 0.1em;
		text-transform: uppercase;
	}
	.name {
		font-weight: 900;
		font-size: 21vw;
		line-height: 0.8;
		letter-spacing: -0.065em;
		margin-left: -0.04em;
	}
	@media (min-width: 768px) {
		.name {
			font-size: min(16.5vw, 14rem);
		}
	}
	.quote {
		font-size: clamp(2.4rem, 6.4vw, 5.25rem);
		line-height: 0.98;
		letter-spacing: -0.01em;
	}
	.quote em {
		font-style: italic;
	}
	.section-head {
		display: flex;
		align-items: flex-end;
		gap: 1rem;
		padding-bottom: 0.75rem;
	}
	.num {
		font-family: 'Instrument Serif', Georgia, serif;
		font-style: italic;
		font-size: clamp(2rem, 5vw, 3.75rem);
		line-height: 0.8;
		padding-bottom: 0.05em;
	}
	.section-title {
		font-weight: 800;
		font-size: clamp(2.5rem, 8vw, 5.5rem);
		line-height: 0.85;
		letter-spacing: -0.05em;
	}
	.row-num {
		font-weight: 300;
		font-size: clamp(2rem, 5vw, 4.5rem);
		line-height: 0.8;
		letter-spacing: -0.05em;
		font-variant-numeric: tabular-nums;
		color: var(--accent);
	}

	/* row inverts on hover */
	.row {
		transition:
			background-color 0.25s ease,
			color 0.25s ease,
			padding 0.25s ease;
	}
	.row:hover,
	.row:focus-visible {
		background: var(--ink);
		color: var(--paper);
	}
	@media (min-width: 768px) {
		.row:hover,
		.row:focus-visible {
			padding-left: 1rem;
			padding-right: 1rem;
		}
	}

	/* red underline sweep */
	.sweep-text {
		background-image: linear-gradient(var(--accent), var(--accent));
		background-size: 0% 2px;
		background-repeat: no-repeat;
		background-position: 0 100%;
		transition: background-size 0.35s cubic-bezier(0.2, 0.7, 0.2, 1);
		padding-bottom: 1px;
	}
	.sweep:hover .sweep-text,
	.sweep:focus-visible .sweep-text {
		background-size: 100% 2px;
	}

	@media (prefers-reduced-motion: reduce) {
		.row,
		.sweep-text {
			transition: none;
		}
	}
</style>
