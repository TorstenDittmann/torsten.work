<script lang="ts">
	const variants = [
		{ slug: 'index', name: 'Index', note: 'Labels on the left, entries on the right.' },
		{ slug: 'serif', name: 'Serif', note: 'One reading column; each entry is a sentence.' },
		{ slug: 'mono', name: 'Mono', note: 'Monospace, aligned like a plain README.' },
		{ slug: 'split', name: 'Split', note: 'Name pinned left, work listed right.' }
	];
</script>

<svelte:head>
	<title>Design variants</title>
	<meta name="robots" content="noindex" />
</svelte:head>

<main class="mx-auto max-w-2xl px-6 py-16 md:py-24">
	<p class="mb-3 font-mono text-sm text-neutral-500">~/torstendittmann/variants</p>
	<h1 class="mb-10 text-3xl font-semibold tracking-tight">Design variants</h1>
	<div class="space-y-1">
		<a
			href="/"
			class="group -mx-4 block rounded-lg px-4 py-4 transition-colors hover:bg-neutral-900/50"
		>
			<span class="font-mono text-neutral-100">00 · current</span>
			<p class="text-sm text-neutral-500">The site as it is today.</p>
		</a>
		{#each variants as v, i (v.slug)}
			<a
				href="/variants/{v.slug}"
				class="group -mx-4 block rounded-lg px-4 py-4 transition-colors hover:bg-neutral-900/50"
			>
				<span class="font-mono text-neutral-100">{String(i + 1).padStart(2, '0')} · {v.name}</span>
				<p class="text-sm text-neutral-500">{v.note}</p>
			</a>
		{/each}
	</div>
</main>
