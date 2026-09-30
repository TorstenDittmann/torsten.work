<script lang="ts">
	import { currentRole, projects, openSource, publications, socials } from '$lib/data';
</script>

<svelte:head>
	<title>Torsten Dittmann</title>
	<link
		href="https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;500&display=swap"
		rel="stylesheet"
	/>
</svelte:head>

<!-- Mono: one monospace face, names aligned in a fixed-width column like a plain README. -->
<div class="page">
	<main>
		<header>
			<h1>Torsten Dittmann</h1>
			<p>{currentRole.role} at {currentRole.company}. Germany.</p>
			<p class="links">
				{#each socials as s (s.name)}<a href={s.url}>{s.name}</a>{/each}
			</p>
		</header>

		<section>
			<h2>projects</h2>
			{#each projects as p (p.name)}
				<div class="row" class:old={p.deprecated}>
					<a href={p.url}>{p.name}</a>
					<span>{p.deprecated ? '(discontinued) ' : ''}{p.description}</span>
				</div>
			{/each}
		</section>

		<section>
			<h2>open source</h2>
			{#each openSource as p (p.name)}
				<div class="row">
					<a href={p.url}>{p.name}</a>
					<span>{p.description}</span>
				</div>
			{/each}
		</section>

		<section>
			<h2>writing</h2>
			{#each publications as p (p.title)}
				<div class="row">
					<span class="year">{p.year}</span>
					<span><a href={p.url}>{p.title}</a> ({p.publication})</span>
				</div>
			{/each}
		</section>
	</main>
</div>

<style>
	.page {
		--bg: #0a0b0b;
		--fg: #d9dcda;
		--muted: #7f8583;
		--line: #2a2e2d;
		min-height: 100vh;
		background: var(--bg);
		color: var(--muted);
		font-family: 'IBM Plex Mono', ui-monospace, monospace;
		font-size: 13.5px;
		line-height: 1.7;
		padding: 0 20px;
	}
	main {
		max-width: 760px;
		margin: 0 auto;
		padding-block: 88px 80px;
	}
	header {
		margin-bottom: 48px;
	}
	h1 {
		color: var(--fg);
		font-size: inherit;
		font-weight: 500;
	}
	.links {
		display: flex;
		gap: 2ch;
		margin-top: 8px;
	}
	section {
		margin-bottom: 40px;
	}
	h2 {
		font-size: inherit;
		font-weight: 400;
		margin-bottom: 8px;
	}
	h2::before {
		content: '# ';
	}
	.row {
		display: grid;
		grid-template-columns: 27ch 1fr;
		gap: 2ch;
		padding-block: 2px;
	}
	.year {
		color: var(--muted);
	}
	.row:has(.year) {
		grid-template-columns: 6ch 1fr;
	}
	a {
		color: var(--fg);
		text-decoration: none;
	}
	a:hover {
		text-decoration: underline;
		text-underline-offset: 3px;
	}
	a:focus-visible {
		outline: 1px solid var(--fg);
		outline-offset: 2px;
	}
	.old a {
		color: var(--muted);
	}
	@media (max-width: 640px) {
		main {
			padding-block: 56px;
		}
		.row,
		.row:has(.year) {
			grid-template-columns: 1fr;
			gap: 0;
			padding-block: 4px;
		}
	}
</style>
