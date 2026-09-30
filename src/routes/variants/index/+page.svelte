<script lang="ts">
	import { currentRole, projects, openSource, publications, socials } from '$lib/data';
</script>

<svelte:head>
	<title>Torsten Dittmann</title>
	<link
		href="https://fonts.googleapis.com/css2?family=Geist:wght@400;500&display=swap"
		rel="stylesheet"
	/>
</svelte:head>

<!-- Index: one sans face, labels in a narrow left column, entries on the right. -->
<div class="page">
	<main>
		<header>
			<h1>Torsten Dittmann</h1>
			<p>{currentRole.role} at {currentRole.company}, based in Germany.</p>
		</header>

		<section>
			<h2>Projects</h2>
			<ul>
				{#each projects as p (p.name)}
					<li class:old={p.deprecated}>
						<a href={p.url}>{p.name}</a>
						<span>{p.description}{p.deprecated ? ' Discontinued.' : ''}</span>
					</li>
				{/each}
			</ul>
		</section>

		<section>
			<h2>Open source</h2>
			<ul>
				{#each openSource as p (p.name)}
					<li>
						<a href={p.url}>{p.name}</a>
						<span>{p.description}</span>
					</li>
				{/each}
			</ul>
		</section>

		<section>
			<h2>Writing</h2>
			<ul>
				{#each publications as p (p.title)}
					<li>
						<a href={p.url}>{p.title}</a>
						<span>{p.publication}, {p.year}</span>
					</li>
				{/each}
			</ul>
		</section>

		<section>
			<h2>Elsewhere</h2>
			<ul class="inline">
				{#each socials as s (s.name)}
					<li><a href={s.url}>{s.name}</a></li>
				{/each}
			</ul>
		</section>
	</main>
</div>

<style>
	.page {
		--bg: #0c0c0d;
		--fg: #e6e6e3;
		--muted: #8b8b87;
		--line: #232325;
		min-height: 100vh;
		background: var(--bg);
		color: var(--fg);
		font-family: 'Geist', system-ui, sans-serif;
		font-size: 15px;
		line-height: 1.6;
		padding: 0 20px;
	}
	main {
		max-width: 640px;
		margin: 0 auto;
		padding-block: 96px 80px;
	}
	header {
		margin-bottom: 56px;
	}
	h1 {
		font-size: 15px;
		font-weight: 500;
	}
	header p {
		color: var(--muted);
	}
	section {
		display: grid;
		grid-template-columns: 8.5rem 1fr;
		gap: 0 16px;
		padding-block: 20px;
		border-top: 1px solid var(--line);
	}
	h2 {
		font-size: 15px;
		font-weight: 400;
		color: var(--muted);
	}
	ul {
		list-style: none;
		margin: 0;
		padding: 0;
		display: grid;
		gap: 12px;
	}
	li {
		display: grid;
	}
	li span {
		color: var(--muted);
	}
	.inline {
		display: flex;
		gap: 20px;
	}
	a {
		color: var(--fg);
		text-decoration: underline;
		text-decoration-color: var(--line);
		text-underline-offset: 3px;
		transition: text-decoration-color 0.15s;
	}
	a:hover {
		text-decoration-color: var(--fg);
	}
	a:focus-visible {
		outline: 1px solid var(--fg);
		outline-offset: 3px;
	}
	.old a {
		color: var(--muted);
	}
	@media (max-width: 560px) {
		main {
			padding-block: 56px;
		}
		section {
			grid-template-columns: 1fr;
			gap: 10px;
		}
	}
</style>
