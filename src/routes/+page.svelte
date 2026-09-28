<script lang="ts">
	const currentRole = {
		company: 'Appwrite',
		role: 'Product Architect'
	};

	const openSource = [
		{
			name: 'appwrite/appwrite',
			description: 'Build like a team of hundreds.',
			url: 'https://github.com/appwrite/appwrite'
		},
		{
			name: 'svelte-markdoc-preprocess',
			description: 'Seamlessly integrate Markdoc into your Svelte applications.',
			url: 'https://github.com/TorstenDittmann/svelte-markdoc-preprocess'
		},
		{
			name: 'docia',
			description:
				'A static documentation generator that prioritizes readability, search engines, and AI assistants.',
			url: 'https://github.com/TorstenDittmann/docia'
		},
		{
			name: 'launch.css',
			description: 'A classless CSS framework for rapid prototyping.',
			url: 'https://github.com/TorstenDittmann/launch.css'
		},
		{
			name: 'workspace-utils',
			description:
				'CLI tool to orchestrate scripts across monorepo workspaces with parallel execution.',
			url: 'https://github.com/TorstenDittmann/workspace-utils'
		},
		{
			name: 'bun-svelte-spa',
			description: 'A simple Svelte framework for Bun.',
			url: 'https://github.com/TorstenDittmann/bun-svelte-spa'
		},
	];

	const publications = [
		{
			title: 'Lessons learned from building a WebSocket server',
			publication: 'php[architect]',
			year: '2021',
			url: 'https://www.phparch.com/article/lessons-learned-from-building-a-websocket-server/'
		}
	];

	const projects = [
		{
			name: 'hansi.codes',
			description:
				'Bring-your-own-key AI code review for GitHub that runs on your own model key.',
			url: 'https://hansi.codes',
			deprecated: false
		},
		{
			name: 'fictioneer.app',
			description: 'Distraction-free writing app for novelists with AI-powered assistance.',
			url: 'https://fictioneer.app',
			deprecated: false
		},
		{
			name: 'pkg.vc',
			description: 'Preview, test, and share npm packages from pull requests.',
			url: 'https://pkg.vc',
			deprecated: false
		},
		{
			name: 'yeet.page',
			description: 'Instant static site preview deployments from the command line.',
			url: 'https://yeet.page',
			deprecated: false
		},
		{
			name: 'OmniaWrite',
			description: 'A text editor for creative writing.',
			url: 'https://github.com/TorstenDittmann/OmniaWrite',
			deprecated: true
		}
	];
</script>

<svelte:head>
	<title>Torsten Dittmann - Product Architect</title>
	<meta
		name="description"
		content="Product Architect at Appwrite. Building developer tools and open source."
	/>

	<!-- Open Graph -->
	<meta property="og:type" content="website" />
	<meta property="og:title" content="Torsten Dittmann" />
	<meta
		property="og:description"
		content="Product Architect at Appwrite. Building developer tools and open source."
	/>
	<meta property="og:url" content="https://torstendittmann.com" />
	<meta property="og:site_name" content="Torsten Dittmann" />
	<meta property="og:locale" content="en_US" />

	<!-- Twitter -->
	<meta name="twitter:card" content="summary" />
	<meta name="twitter:site" content="@DittmannTorsten" />
	<meta name="twitter:creator" content="@DittmannTorsten" />
	<meta name="twitter:title" content="Torsten Dittmann" />
	<meta
		name="twitter:description"
		content="Product Architect at Appwrite. Building developer tools and open source."
	/>

	<!-- Additional SEO -->
	<meta name="author" content="Torsten Dittmann" />
	<meta name="robots" content="index, follow" />
	<link rel="canonical" href="https://torstendittmann.com" />
</svelte:head>

<main class="mx-auto max-w-2xl px-6 py-16 md:py-24">
	<!-- Header -->
	<header class="mb-20">
		<p class="mb-3 font-mono text-sm text-neutral-600">~/torstendittmann</p>
		<h1 class="mb-4 text-3xl font-semibold tracking-tight text-neutral-100">Torsten Dittmann</h1>
		<p class="mb-6 leading-relaxed text-neutral-400">Product Architect from Germany.</p>
		<nav class="flex flex-wrap gap-x-6 gap-y-2 text-sm">
			<a href="https://github.com/TorstenDittmann" class="group flex items-center gap-1">
				GitHub
				<span
					class="inline-block transition-transform group-hover:translate-x-0.5 group-hover:-translate-y-0.5"
					>↗</span
				>
			</a>
			<a href="https://linkedin.com/in/torstendittmann" class="group flex items-center gap-1">
				LinkedIn
				<span
					class="inline-block transition-transform group-hover:translate-x-0.5 group-hover:-translate-y-0.5"
					>↗</span
				>
			</a>
			<a href="https://x.com/DittmannTorsten" class="group flex items-center gap-1">
				X
				<span
					class="inline-block transition-transform group-hover:translate-x-0.5 group-hover:-translate-y-0.5"
					>↗</span
				>
			</a>
		</nav>
	</header>

	<!-- Currently -->
	<section class="mb-20">
		<h2 class="mb-4 text-sm font-medium tracking-wider text-neutral-500 uppercase">Currently</h2>
		<div class="flex items-center gap-3">
			<span class="relative flex h-2 w-2">
				<span
					class="absolute inline-flex h-full w-full animate-ping rounded-full bg-emerald-400 opacity-75"
				></span>
				<span class="relative inline-flex h-2 w-2 rounded-full bg-emerald-500"></span>
			</span>
			<p class="text-neutral-300">
				{currentRole.role} at <span class="text-neutral-100">{currentRole.company}</span>
			</p>
		</div>
	</section>

	<!-- Projects -->
	<section class="mb-20">
		<h2 class="mb-8 text-sm font-medium tracking-wider text-neutral-500 uppercase">Projects</h2>
		<div class="space-y-1">
			{#each projects as project (project.name)}
				<a
					href={project.url}
					class="group -mx-4 block rounded-lg px-4 py-4 transition-colors hover:bg-neutral-900/50"
				>
					<div class="mb-1 flex items-center gap-2">
						<span class="font-medium text-neutral-100 group-hover:text-white">{project.name}</span>
						{#if project.deprecated}
							<span class="rounded bg-neutral-800 px-1.5 py-0.5 text-xs text-neutral-500"
								>deprecated</span
							>
						{/if}
						<span
							class="ml-auto text-neutral-600 transition-transform group-hover:translate-x-0.5 group-hover:-translate-y-0.5 group-hover:text-neutral-400"
							>↗</span
						>
					</div>
					<p class="text-sm text-neutral-500">{project.description}</p>
				</a>
			{/each}
		</div>
	</section>

	<!-- Open Source -->
	<section class="mb-20">
		<h2 class="mb-8 text-sm font-medium tracking-wider text-neutral-500 uppercase">Open Source</h2>
		<div class="space-y-1">
			{#each openSource as project (project.name)}
				<a
					href={project.url}
					class="group -mx-4 block rounded-lg px-4 py-4 transition-colors hover:bg-neutral-900/50"
				>
					<div class="mb-1 flex items-center gap-2">
						<span class="font-mono text-neutral-100 group-hover:text-white">{project.name}</span>
						<span
							class="ml-auto text-neutral-600 transition-transform group-hover:translate-x-0.5 group-hover:-translate-y-0.5 group-hover:text-neutral-400"
							>↗</span
						>
					</div>
					<p class="text-sm text-neutral-500">{project.description}</p>
				</a>
			{/each}
		</div>
	</section>

	<!-- Publications -->
	<section class="mb-20">
		<h2 class="mb-8 text-sm font-medium tracking-wider text-neutral-500 uppercase">Writing</h2>
		<div class="space-y-1">
			{#each publications as pub (pub.title)}
				<a
					href={pub.url}
					class="group -mx-4 flex flex-wrap items-baseline justify-between gap-2 rounded-lg px-4 py-3 transition-colors hover:bg-neutral-900/50"
				>
					<span class="text-neutral-100 group-hover:text-white">{pub.title}</span>
					<span class="text-sm text-neutral-600">{pub.publication} · {pub.year}</span>
				</a>
			{/each}
		</div>
	</section>

	<!-- Footer -->
	<footer class="border-t border-neutral-800 pt-8">
		<p class="font-mono text-sm text-neutral-600">
			<span class="text-neutral-500">$</span> echo "Thanks for visiting"
		</p>
	</footer>
</main>
