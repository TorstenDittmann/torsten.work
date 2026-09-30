<script lang="ts">
	import { currentRole, projects, openSource, publications, socials } from '$lib/data';
</script>

<svelte:head>
	<title>Torsten Dittmann</title>
	<link
		href="https://fonts.googleapis.com/css2?family=Hanken+Grotesk:wght@400;500&display=swap"
		rel="stylesheet"
	/>
</svelte:head>

<!-- Split: identity pinned on the left on wide screens, work listed on the right. -->
<div class="page">
	<div class="wrap">
		<aside>
			<h1>Torsten Dittmann</h1>
			<p>{currentRole.role} at {currentRole.company}.<br />Germany.</p>
			<ul>
				{#each socials as s (s.name)}
					<li><a href={s.url}>{s.name}</a></li>
				{/each}
			</ul>
		</aside>

		<main>
			<section>
				<h2>Projects</h2>
				{#each projects as p (p.name)}
					<a class="item" class:old={p.deprecated} href={p.url}>
						<span class="name">{p.name}{p.deprecated ? ' (discontinued)' : ''}</span>
						<span class="desc">{p.description}</span>
					</a>
				{/each}
			</section>

			<section>
				<h2>Open source</h2>
				{#each openSource as p (p.name)}
					<a class="item" href={p.url}>
						<span class="name">{p.name}</span>
						<span class="desc">{p.description}</span>
					</a>
				{/each}
			</section>

			<section>
				<h2>Writing</h2>
				{#each publications as p (p.title)}
					<a class="item" href={p.url}>
						<span class="name">{p.title}</span>
						<span class="desc">{p.publication}, {p.year}</span>
					</a>
				{/each}
			</section>
		</main>
	</div>
</div>

<style>
	.page {
		--bg: #0b0c0e;
		--fg: #e4e6ea;
		--muted: #868b94;
		--hover: #15171a;
		min-height: 100vh;
		background: var(--bg);
		color: var(--fg);
		font-family: 'Hanken Grotesk', system-ui, sans-serif;
		font-size: 15px;
		line-height: 1.55;
		padding: 0 20px;
	}
	.wrap {
		max-width: 960px;
		margin: 0 auto;
		display: grid;
		grid-template-columns: 240px 1fr;
		gap: 64px;
		padding-block: 96px 80px;
	}
	aside {
		position: sticky;
		top: 96px;
		align-self: start;
	}
	h1 {
		font-size: 15px;
		font-weight: 500;
	}
	aside p {
		color: var(--muted);
		margin-bottom: 20px;
	}
	aside ul {
		list-style: none;
		margin: 0;
		padding: 0;
		display: grid;
		gap: 4px;
	}
	aside a {
		color: var(--muted);
		text-decoration: none;
	}
	aside a:hover {
		color: var(--fg);
	}
	main {
		min-width: 0;
		display: grid;
		gap: 44px;
	}
	h2 {
		font-size: 15px;
		font-weight: 400;
		color: var(--muted);
		margin-bottom: 6px;
	}
	.item {
		display: grid;
		margin-inline: -12px;
		padding: 10px 12px;
		border-radius: 6px;
		color: var(--fg);
		text-decoration: none;
		transition: background-color 0.15s;
	}
	.item:hover {
		background: var(--hover);
	}
	.name {
		font-weight: 500;
	}
	.desc {
		color: var(--muted);
	}
	.old .name {
		color: var(--muted);
		font-weight: 400;
	}
	a:focus-visible {
		outline: 1px solid var(--fg);
		outline-offset: 2px;
	}
	@media (max-width: 760px) {
		.wrap {
			grid-template-columns: 1fr;
			gap: 40px;
			padding-block: 56px;
		}
		aside {
			position: static;
		}
		aside ul {
			display: flex;
			gap: 16px;
		}
	}
</style>
