<script lang="ts">
	import { currentRole, projects, openSource, publications, socials } from '$lib/data';
</script>

<svelte:head>
	<title>Torsten Dittmann</title>
	<link
		href="https://fonts.googleapis.com/css2?family=Newsreader:ital,opsz,wght@0,6..72,400;0,6..72,500;1,6..72,400&display=swap"
		rel="stylesheet"
	/>
</svelte:head>

<!-- Serif: a single reading column; each entry reads as one sentence. -->
<div class="page">
	<main>
		<h1>Torsten Dittmann</h1>
		<p class="lede">
			{currentRole.role} at {currentRole.company}, based in Germany. I build developer tools and
			write open source.
		</p>

		<h2>Projects</h2>
		{#each projects as p (p.name)}
			<p class:old={p.deprecated}>
				<a href={p.url}>{p.name}</a>. {p.description}
				{#if p.deprecated}<em>No longer maintained.</em>{/if}
			</p>
		{/each}

		<h2>Open source</h2>
		{#each openSource as p (p.name)}
			<p><a href={p.url}>{p.name}</a>. {p.description}</p>
		{/each}

		<h2>Writing</h2>
		{#each publications as p (p.title)}
			<p><a href={p.url}>{p.title}</a>, <em>{p.publication}</em>, {p.year}.</p>
		{/each}

		<footer>
			{#each socials as s (s.name)}<a href={s.url}>{s.name}</a>{/each}
		</footer>
	</main>
</div>

<style>
	.page {
		--bg: #0f0e0d;
		--fg: #e9e5de;
		--muted: #97918a;
		--line: #3a3632;
		min-height: 100vh;
		background: var(--bg);
		color: var(--muted);
		font-family: 'Newsreader', Georgia, serif;
		font-size: 19px;
		line-height: 1.55;
		padding: 0 20px;
	}
	main {
		max-width: 34rem;
		margin: 0 auto;
		padding-block: 104px 88px;
	}
	h1 {
		color: var(--fg);
		font-size: 19px;
		font-weight: 500;
	}
	.lede {
		margin-bottom: 48px;
	}
	h2 {
		color: var(--fg);
		font-size: 19px;
		font-weight: 400;
		font-style: italic;
		margin: 40px 0 12px;
	}
	p {
		margin: 0 0 10px;
	}
	a {
		color: var(--fg);
		text-decoration: underline;
		text-decoration-color: var(--line);
		text-decoration-thickness: 1px;
		text-underline-offset: 4px;
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
	footer {
		display: flex;
		gap: 20px;
		margin-top: 56px;
	}
	@media (max-width: 560px) {
		.page {
			font-size: 18px;
		}
		main {
			padding-block: 56px;
		}
		h1,
		h2 {
			font-size: 18px;
		}
	}
</style>
