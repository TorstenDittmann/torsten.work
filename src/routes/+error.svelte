<script lang="ts">
	import { page } from '$app/state';
</script>

<svelte:head>
	<title>404 - Page not found</title>
</svelte:head>

<main class="mx-auto flex min-h-screen max-w-2xl flex-col items-center justify-center px-6">
	<div class="text-center">
		<p class="mb-4 font-mono text-neutral-600">$ cd {page.url.pathname}</p>
		<h1 class="mb-2 font-mono text-6xl font-bold text-neutral-100">{page.status}</h1>
		<p class="mb-8 font-mono text-neutral-500">
			{#if page.status === 404}
				bash: cd: {page.url.pathname}: No such file or directory
			{:else}
				{page.error?.message}
			{/if}
		</p>
		<a
			href="/"
			class="inline-flex items-center gap-2 font-mono text-sm text-neutral-400 transition-colors hover:text-neutral-100"
		>
			<span>$</span> cd ~
		</a>
	</div>
</main>
