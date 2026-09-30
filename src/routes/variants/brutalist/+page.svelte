<script lang="ts">
	import { currentRole, openSource, publications, projects, socials } from '$lib/data';

	const cardColors = ['#ffe14d', '#ff6ec7', '#4d7cff', '#b6f24a', '#ff8a3d'];
	const tilts = ['-rotate-1', 'rotate-1', '-rotate-[0.6deg]', 'rotate-[0.8deg]', '-rotate-1'];
	const ticker = [...projects.map((p) => p.name), ...openSource.map((o) => o.name)];
	const year = new Date().getFullYear();
</script>

<svelte:head>
	<title>Torsten Dittmann — brutalist</title>
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous" />
	<link
		href="https://fonts.googleapis.com/css2?family=Archivo+Black&family=Space+Grotesk:wght@400;500;700&display=swap"
		rel="stylesheet"
	/>
</svelte:head>

<div class="page min-h-screen overflow-x-hidden bg-[#fff6e0] text-black">
	<!-- Top bar -->
	<div class="border-b-4 border-black bg-black text-[#fff6e0]">
		<div
			class="mx-auto flex max-w-6xl items-center justify-between gap-4 px-4 py-2 text-xs font-bold tracking-widest uppercase sm:px-6"
		>
			<span>torsten.work</span>
			<span class="flex items-center gap-2">
				<span class="blink inline-block size-2.5 rounded-full bg-[#b6f24a]" aria-hidden="true"
				></span>
				Online &amp; building
			</span>
		</div>
	</div>

	<main class="mx-auto max-w-6xl px-4 pt-10 pb-8 sm:px-6 sm:pt-16">
		<!-- Hero -->
		<header class="relative mb-14 sm:mb-20">
			<div class="relative inline-block">
				<span
					class="sticker relative z-10 mb-4 rotate-3 bg-[#ff6ec7] sm:absolute sm:-top-6 sm:-right-24 sm:mb-0 sm:rotate-6"
				>
					Made in Germany
				</span>
				<h1
					class="display pr-2 text-[3.4rem] leading-[0.88] tracking-tight uppercase sm:text-8xl lg:text-[9.5rem]"
				>
					Torsten<br />
					<span class="relative inline-block">
						<span
							class="absolute inset-x-[-0.15em] bottom-[0.08em] -z-0 h-[0.38em] -rotate-1 bg-[#ffe14d]"
							aria-hidden="true"
						></span>
						<span class="relative">Dittmann</span>
					</span>
				</h1>
			</div>

			<div class="mt-8 flex flex-col gap-6 sm:mt-10 md:flex-row md:items-end md:justify-between">
				<p class="max-w-xl text-2xl leading-tight font-bold sm:text-3xl lg:text-4xl">
					Product Architect<br class="hidden sm:block" /> from Germany.
				</p>

				<nav aria-label="Social links">
					<ul class="flex flex-wrap gap-3">
						{#each socials as social (social.url)}
							<li>
								<a
									href={social.url}
									class="btn bg-white px-4 py-2 text-base font-bold"
									target="_blank"
									rel="noopener noreferrer"
								>
									{social.name} <span aria-hidden="true">↗</span>
								</a>
							</li>
						{/each}
					</ul>
				</nav>
			</div>
		</header>

		<!-- Currently -->
		<section aria-labelledby="currently" class="relative mb-16 sm:mb-24">
			<div
				class="box relative flex flex-col gap-4 bg-[#4d7cff] p-6 text-white sm:flex-row sm:items-center sm:justify-between sm:p-10"
			>
				<div>
					<h2 id="currently" class="mb-2 text-sm font-bold tracking-[0.25em] uppercase">
						Currently @
					</h2>
					<p class="display text-5xl leading-none uppercase sm:text-7xl">
						{currentRole.company}
					</p>
				</div>
				<p
					class="w-fit -rotate-2 border-4 border-black bg-white px-4 py-2 text-lg font-bold text-black shadow-[4px_4px_0_#000] sm:text-xl"
				>
					{currentRole.role}
				</p>
			</div>
			<span class="sticker absolute -right-2 -bottom-5 -rotate-6 bg-[#b6f24a] sm:right-10">
				Open for ideas
			</span>
		</section>
	</main>

	<!-- Marquee -->
	<div
		class="marquee relative mb-16 -rotate-1 overflow-hidden border-y-4 border-black bg-[#ffe14d] py-3 sm:mb-24"
		aria-hidden="true"
	>
		<div class="track flex w-max">
			{#each [0, 1] as dup (dup)}
				<div class="flex shrink-0 items-center">
					{#each ticker as name (name)}
						<span class="display px-5 text-2xl whitespace-nowrap uppercase sm:text-4xl">{name}</span
						>
						<span class="text-2xl sm:text-4xl">✺</span>
					{/each}
				</div>
			{/each}
		</div>
	</div>

	<div class="mx-auto max-w-6xl px-4 pb-16 sm:px-6">
		<!-- Projects -->
		<section aria-labelledby="projects" class="mb-20 sm:mb-28">
			<div class="mb-8 flex items-end justify-between gap-4">
				<h2 id="projects" class="display text-4xl uppercase sm:text-6xl">Projects</h2>
				<span class="mb-1 text-sm font-bold tracking-widest uppercase"
					>{projects.length} things shipped</span
				>
			</div>

			<ul class="grid gap-6 sm:grid-cols-2 lg:grid-cols-3">
				{#each projects as project, i (project.url)}
					<li
						class="{project.deprecated ? '' : tilts[i % tilts.length]} {i === 0
							? 'lg:col-span-2'
							: ''}"
					>
						<a
							href={project.url}
							target="_blank"
							rel="noopener noreferrer"
							class="card group relative flex h-full flex-col overflow-hidden p-6 {project.deprecated
								? 'pb-20'
								: ''}"
							class:rip={project.deprecated}
							style:background-color={project.deprecated
								? '#d9d4c7'
								: cardColors[i % cardColors.length]}
						>
							<div class="mb-10 flex items-start justify-between gap-3">
								<span
									class="border-[3px] border-black bg-white px-2 py-0.5 text-xs font-bold tracking-widest"
								>
									#{String(i + 1).padStart(2, '0')}
								</span>
								<span
									class="grid size-10 shrink-0 place-items-center border-[3px] border-black bg-white text-xl font-bold transition-transform group-hover:rotate-45"
									aria-hidden="true">↗</span
								>
							</div>
							<h3
								class="display mb-2 break-words {i === 0 ? 'text-3xl lg:text-5xl' : 'text-3xl'}"
								class:line-through={project.deprecated}
								class:decoration-4={project.deprecated}
							>
								{project.name}
							</h3>
							<p class="text-base leading-snug font-medium">{project.description}</p>
							{#if project.deprecated}
								<span class="sr-only">(deprecated)</span>
								<span class="tape" aria-hidden="true">RIP · deprecated · RIP</span>
							{/if}
						</a>
					</li>
				{/each}
			</ul>
		</section>

		<!-- Open source -->
		<section aria-labelledby="oss" class="mb-20 sm:mb-28">
			<div class="mb-8 flex flex-wrap items-center gap-4">
				<h2 id="oss" class="display text-4xl uppercase sm:text-6xl">Open Source</h2>
				<span class="sticker rotate-3 bg-[#ff8a3d]">Free as in beer</span>
			</div>

			<ul class="box divide-y-4 divide-black overflow-hidden bg-white">
				{#each openSource as repo (repo.url)}
					<li>
						<a
							href={repo.url}
							target="_blank"
							rel="noopener noreferrer"
							class="row group flex flex-col gap-1 p-5 sm:flex-row sm:items-center sm:gap-6 sm:p-6"
						>
							<span class="font-mono text-lg font-bold sm:w-72 sm:shrink-0">{repo.name}</span>
							<span class="flex-1 leading-snug">{repo.description}</span>
							<span
								class="hidden text-2xl font-bold transition-transform group-hover:translate-x-1 sm:inline"
								aria-hidden="true">→</span
							>
						</a>
					</li>
				{/each}
			</ul>
		</section>

		<!-- Writing -->
		<section aria-labelledby="writing" class="mb-20 sm:mb-28">
			<h2 id="writing" class="display mb-8 text-4xl uppercase sm:text-6xl">Writing</h2>
			<ul class="grid gap-6 {publications.length > 1 ? 'md:grid-cols-2' : ''}">
				{#each publications as pub (pub.url)}
					<li>
						<a
							href={pub.url}
							target="_blank"
							rel="noopener noreferrer"
							class="card flex h-full flex-col bg-[#ff6ec7] p-6 sm:p-8"
						>
							<span class="mb-6 flex flex-wrap gap-2 text-xs font-bold tracking-widest uppercase">
								<span class="border-[3px] border-black bg-white px-2 py-0.5">{pub.publication}</span
								>
								<span class="border-[3px] border-black bg-black px-2 py-0.5 text-white"
									>{pub.year}</span
								>
							</span>
							<span class="display max-w-3xl text-2xl leading-tight sm:text-4xl">{pub.title}</span>
							<span class="mt-6 text-sm font-bold tracking-widest uppercase"
								>Read article <span aria-hidden="true">↗</span></span
							>
						</a>
					</li>
				{/each}
			</ul>
		</section>

		<!-- Say hi -->
		<section
			aria-labelledby="hi"
			class="box relative flex flex-col gap-6 bg-[#b6f24a] p-6 sm:p-10 md:flex-row md:items-center md:justify-between"
		>
			<h2 id="hi" class="display text-4xl leading-none uppercase sm:text-6xl">
				Say hi <span aria-hidden="true">✺</span>
			</h2>
			<ul class="flex flex-wrap gap-3">
				{#each socials as social (social.url)}
					<li>
						<a
							href={social.url}
							class="btn bg-white px-5 py-3 text-lg font-bold"
							target="_blank"
							rel="noopener noreferrer"
						>
							{social.name} <span aria-hidden="true">↗</span>
						</a>
					</li>
				{/each}
			</ul>
		</section>
	</div>

	<footer class="border-t-4 border-black bg-black text-[#fff6e0]">
		<div
			class="mx-auto flex max-w-6xl flex-col gap-2 px-4 py-6 text-sm font-bold tracking-widest uppercase sm:flex-row sm:justify-between sm:px-6"
		>
			<span>© {year} Torsten Dittmann</span>
			<span>Handmade with thick borders</span>
		</div>
	</footer>
</div>

<style>
	.page {
		font-family: 'Space Grotesk', system-ui, sans-serif;
	}
	.display {
		font-family: 'Archivo Black', 'Space Grotesk', system-ui, sans-serif;
		font-weight: 400;
	}
	.page a {
		color: #000;
		text-decoration: none;
	}
	.page a:focus-visible {
		outline: 4px solid #4d7cff;
		outline-offset: 4px;
	}

	.box {
		border: 4px solid #000;
		box-shadow: 8px 8px 0 #000;
	}

	.sticker {
		display: inline-block;
		border: 3px solid #000;
		padding: 0.35rem 0.8rem;
		font-size: 0.8rem;
		font-weight: 700;
		letter-spacing: 0.12em;
		text-transform: uppercase;
		white-space: nowrap;
		box-shadow: 3px 3px 0 #000;
		color: #000;
	}

	.btn,
	.card {
		border: 4px solid #000;
		box-shadow: 6px 6px 0 #000;
		transition:
			transform 120ms ease,
			box-shadow 120ms ease;
	}
	.btn {
		display: inline-flex;
		gap: 0.4rem;
		box-shadow: 4px 4px 0 #000;
	}
	.btn:hover,
	.btn:focus-visible {
		transform: translate(2px, 2px);
		box-shadow: 2px 2px 0 #000;
		background: #ffe14d;
	}
	.card:hover,
	.card:focus-visible {
		transform: translate(3px, 3px);
		box-shadow: 3px 3px 0 #000;
	}
	.btn:active,
	.card:active {
		transform: translate(6px, 6px);
		box-shadow: 0 0 0 #000;
	}

	.row {
		transition: background-color 120ms ease;
	}
	.row:hover,
	.row:focus-visible {
		background: #b6f24a;
	}
	.row:focus-visible {
		outline-offset: -6px;
	}

	.rip {
		color: #333;
	}
	.tape {
		position: absolute;
		right: -3rem;
		bottom: 1.4rem;
		transform: rotate(-8deg);
		background: #000;
		color: #ffe14d;
		padding: 0.35rem 3.5rem;
		font-family: 'Archivo Black', sans-serif;
		font-size: 0.85rem;
		letter-spacing: 0.15em;
		text-transform: uppercase;
		white-space: nowrap;
		pointer-events: none;
	}

	.marquee {
		margin-left: -1rem;
		margin-right: -1rem;
	}
	.track {
		animation: scroll 30s linear infinite;
	}
	@keyframes scroll {
		to {
			transform: translateX(-50%);
		}
	}
	.blink {
		animation: blink 1.4s steps(2, start) infinite;
	}
	@keyframes blink {
		to {
			visibility: hidden;
		}
	}

	@media (prefers-reduced-motion: reduce) {
		.track,
		.blink {
			animation: none;
		}
		.btn,
		.card,
		.row {
			transition: none;
		}
	}
</style>
