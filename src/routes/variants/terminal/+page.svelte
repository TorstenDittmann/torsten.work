<script lang="ts">
	import { currentRole, projects, openSource, publications, socials } from '$lib/data';

	const bannerWide =
		"  _____              _               ____  _ _   _\n |_   _|__  _ __ ___| |_ ___ _ __   |  _ \\(_) |_| |_ _ __ ___   __ _ _ __  _ __\n   | |/ _ \\| '__/ __| __/ _ \\ '_ \\  | | | | | __| __| '_ ` _ \\ / _` | '_ \\| '_ \\\n   | | (_) | |  \\__ \\ ||  __/ | | | | |_| | | |_| |_| | | | | | (_| | | | | | | |\n   |_|\\___/|_|  |___/\\__\\___|_| |_| |____/|_|\\__|\\__|_| |_| |_|\\__,_|_| |_|_| |_|";
	const bannerNarrow =
		"  _____           _\n |_   _|__ _ _ __| |_ ___ _ _\n   | |/ _ \\ '_(_-<  _/ -_) ' \\\n   |_|\\___/_| /__/\\__\\___|_||_|\n  ___  _ _   _\n |   \\(_) |_| |_ _ __  __ _ _ _  _ _\n | |) | |  _|  _| '  \\/ _` | ' \\| ' \\\n |___/|_|\\__|\\__|_|_|_\\__,_|_||_|_||_|";

	// Deterministic short "commit hash" derived from the title (decorative only).
	function shortHash(input: string): string {
		let h = 0x811c9dc5;
		for (let i = 0; i < input.length; i++) {
			h ^= input.charCodeAt(i);
			h = Math.imul(h, 0x01000193);
		}
		return (h >>> 0).toString(16).padStart(8, '0').slice(0, 7);
	}

	const host = (url: string) => url.replace(/^https?:\/\//, '').replace(/\/$/, '');
</script>

<svelte:head>
	<title>Torsten Dittmann — terminal</title>
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous" />
	<link href="https://fonts.googleapis.com/css2?family=VT323&display=swap" rel="stylesheet" />
</svelte:head>

<div class="crt min-h-screen">
	<div class="overlay scanlines" aria-hidden="true"></div>
	<div class="overlay vignette" aria-hidden="true"></div>

	<main class="screen mx-auto max-w-5xl px-4 py-6 sm:px-8 sm:py-12">
		<div class="window">
			<div class="titlebar" aria-hidden="true">
				<span class="dots"><i></i><i></i><i></i></span>
				<span class="truncate">torsten@work: ~ — zsh — 80×24</span>
			</div>

			<div class="session">
				<section class="block" style="--i: 0" aria-label="Banner">
					<p class="dim">Last login: ttys001 from germany</p>
					<pre class="banner banner-wide" aria-hidden="true">{bannerWide}</pre>
					<pre class="banner banner-narrow" aria-hidden="true">{bannerNarrow}</pre>
				</section>

				<section class="block" style="--i: 1" aria-labelledby="whoami">
					<p class="prompt-line">
						<span class="ps1">torsten<span class="host">@work</span><span class="dim">:</span><span class="path">~</span>$</span>
						<span class="cmd" style="--n: 6">whoami</span>
					</p>
					<div class="out">
						<h1 id="whoami" class="name glow">Torsten Dittmann</h1>
						<p>Product Architect from Germany.</p>
					</div>
				</section>

				<section class="block" style="--i: 2" aria-labelledby="currently">
					<p class="prompt-line">
						<span class="ps1">torsten<span class="host">@work</span><span class="dim">:</span><span class="path">~</span>$</span>
						<span class="cmd" style="--n: 17">cat currently.txt</span>
					</p>
					<div class="out">
						<h2 id="currently" class="sr-only">Currently</h2>
						<p>
							<span class="dim">role&nbsp;&nbsp;&nbsp;:</span>
							<span class="hi">{currentRole.role}</span>
						</p>
						<p>
							<span class="dim">company:</span>
							<span class="hi">{currentRole.company}</span>
						</p>
					</div>
				</section>

				<section class="block" style="--i: 3" aria-labelledby="projects">
					<p class="prompt-line">
						<span class="ps1">torsten<span class="host">@work</span><span class="dim">:</span><span class="path">~</span>$</span>
						<span class="cmd" style="--n: 17">ls -la ~/projects</span>
					</p>
					<div class="out">
						<h2 id="projects" class="sr-only">Projects</h2>
						<p class="dim">total {projects.length}</p>
						<ul class="ls">
							{#each projects as project (project.name)}
								<li class="ls-row" class:deprecated={project.deprecated}>
									<span class="perm">{project.deprecated ? 'dr--r--r--' : 'drwxr-xr-x'}</span>
									<span class="owner dim">torsten</span>
									<span class="entry">
										<a href={project.url} target="_blank" rel="noreferrer">{project.name}/</a>
										{#if project.deprecated}
											<span class="tag">[deprecated]</span>
										{/if}
									</span>
									<span class="desc"><span class="dim">#&nbsp;</span>{project.description}</span>
								</li>
							{/each}
						</ul>
					</div>
				</section>

				<section class="block" style="--i: 4" aria-labelledby="oss">
					<p class="prompt-line">
						<span class="ps1">torsten<span class="host">@work</span><span class="dim">:</span><span class="path">~</span>$</span>
						<span class="cmd" style="--n: 23">ls -la ~/src/open-source</span>
					</p>
					<div class="out">
						<h2 id="oss" class="sr-only">Open Source</h2>
						<p class="dim">total {openSource.length}</p>
						<ul class="ls">
							{#each openSource as repo (repo.name)}
								<li class="ls-row">
									<span class="perm">drwxr-xr-x</span>
									<span class="owner dim">torsten</span>
									<span class="entry">
										<a href={repo.url} target="_blank" rel="noreferrer">{repo.name}</a>
									</span>
									<span class="desc"><span class="dim">#&nbsp;</span>{repo.description}</span>
								</li>
							{/each}
						</ul>
					</div>
				</section>

				<section class="block" style="--i: 5" aria-labelledby="writing">
					<p class="prompt-line">
						<span class="ps1">torsten<span class="host">@work</span><span class="dim">:</span><span class="path">~</span>$</span>
						<span class="cmd" style="--n: 24">git log --oneline writing</span>
					</p>
					<div class="out">
						<h2 id="writing" class="sr-only">Writing</h2>
						<ul>
							{#each publications as pub (pub.url)}
								<li class="log">
									<span class="hash">{shortHash(pub.title)}</span>
									<span class="dim">({pub.year}, {pub.publication})</span>
									<a href={pub.url} target="_blank" rel="noreferrer">{pub.title}</a>
								</li>
							{/each}
						</ul>
					</div>
				</section>

				<section class="block" style="--i: 6" aria-labelledby="socials">
					<p class="prompt-line">
						<span class="ps1">torsten<span class="host">@work</span><span class="dim">:</span><span class="path">~</span>$</span>
						<span class="cmd" style="--n: 13">cat ~/.socials</span>
					</p>
					<div class="out">
						<h2 id="socials" class="sr-only">Elsewhere</h2>
						<ul class="socials">
							{#each socials as social (social.name)}
								<li>
									<span class="dim">{social.name.toLowerCase()} -&gt;</span>
									<a href={social.url} target="_blank" rel="noreferrer">{host(social.url)}</a>
								</li>
							{/each}
						</ul>
					</div>
				</section>

				<p class="block prompt-line final" style="--i: 7">
					<span class="ps1">torsten<span class="host">@work</span><span class="dim">:</span><span class="path">~</span>$</span>
					<span class="cursor" aria-hidden="true"></span>
				</p>
			</div>
		</div>
		<p class="footnote dim">[ phosphor: P1 green · refresh 60Hz · no cookies ]</p>
	</main>
</div>

<style>
	.crt {
		--bg: #030a05;
		--fg: #8dffae;
		--hi: #d4ffe0;
		--dim: #4fbf72;
		--edge: #1c4a2a;
		--amber: #ffc24a;
		--glow: 0 0 2px rgb(120 255 160 / 0.8), 0 0 10px rgb(60 255 120 / 0.35);

		position: relative;
		isolation: isolate;
		background:
			radial-gradient(ellipse at 50% 0%, #0a2412 0%, transparent 60%),
			radial-gradient(ellipse at 50% 100%, #06170b 0%, transparent 70%), var(--bg);
		color: var(--fg);
		font-family: 'VT323', ui-monospace, 'SFMono-Regular', Menlo, Consolas, monospace;
		font-size: 1.3rem;
		line-height: 1.3;
		text-shadow: var(--glow);
		overflow-x: clip;
	}

	@media (min-width: 640px) {
		.crt {
			font-size: 1.45rem;
		}
	}

	.crt ::selection {
		background: var(--fg);
		color: var(--bg);
		text-shadow: none;
	}

	/* ---------- CRT overlays ---------- */
	.overlay {
		position: fixed;
		inset: 0;
		pointer-events: none;
		z-index: 10;
	}

	.scanlines {
		background: repeating-linear-gradient(
			to bottom,
			rgb(0 0 0 / 0) 0px,
			rgb(0 0 0 / 0) 2px,
			rgb(0 0 0 / 0.28) 3px,
			rgb(0 0 0 / 0) 4px
		);
		animation: flicker 6s infinite steps(1);
	}

	.vignette {
		background: radial-gradient(ellipse at center, transparent 55%, rgb(0 0 0 / 0.65) 100%);
	}

	@keyframes flicker {
		0%,
		100% {
			opacity: 1;
		}
		47% {
			opacity: 0.85;
		}
		48% {
			opacity: 1;
		}
		82% {
			opacity: 0.92;
		}
		83% {
			opacity: 1;
		}
	}

	/* ---------- window chrome ---------- */
	.window {
		border: 1px solid var(--edge);
		border-radius: 10px;
		background: rgb(2 12 5 / 0.6);
		box-shadow:
			0 0 0 1px rgb(0 0 0 / 0.6),
			0 0 40px rgb(40 255 110 / 0.08),
			inset 0 0 60px rgb(40 255 110 / 0.04);
		overflow: hidden;
	}

	.titlebar {
		display: flex;
		align-items: center;
		gap: 0.75rem;
		padding: 0.35rem 0.9rem;
		border-bottom: 1px solid var(--edge);
		color: var(--dim);
		font-size: 0.95em;
		background: rgb(20 60 30 / 0.25);
	}

	.dots {
		display: inline-flex;
		gap: 0.4rem;
		flex-shrink: 0;
	}

	.dots i {
		width: 0.6rem;
		height: 0.6rem;
		border-radius: 999px;
		border: 1px solid var(--dim);
	}

	.dots i:first-child {
		background: var(--dim);
	}

	.session {
		padding: 1.25rem 1rem 1.5rem;
		display: grid;
		gap: 1.6rem;
	}

	@media (min-width: 640px) {
		.session {
			padding: 1.75rem 2rem 2.25rem;
		}
	}

	/* ---------- text ---------- */
	.dim {
		color: var(--dim);
	}

	.hi {
		color: var(--hi);
	}

	.glow {
		color: var(--hi);
		text-shadow:
			0 0 2px rgb(200 255 220 / 0.9),
			0 0 14px rgb(60 255 120 / 0.6),
			0 0 30px rgb(60 255 120 / 0.25);
	}

	.name {
		font-size: 2.2em;
		line-height: 1;
		letter-spacing: 0.02em;
		margin-bottom: 0.2rem;
	}

	.ps1 {
		color: var(--dim);
		white-space: nowrap;
	}

	.path {
		color: var(--fg);
	}

	.prompt-line {
		display: flex;
		flex-wrap: wrap;
		column-gap: 0.6ch;
	}

	.cmd {
		color: var(--hi);
		display: inline-block;
		white-space: nowrap;
		overflow: hidden;
		vertical-align: bottom;
	}

	.out {
		margin-top: 0.35rem;
	}

	/* ---------- banner ---------- */
	.banner {
		font-family: inherit;
		line-height: 1;
		color: var(--hi);
		text-shadow:
			0 0 2px rgb(200 255 220 / 0.9),
			0 0 12px rgb(60 255 120 / 0.55);
		margin: 0.5rem 0 0;
		white-space: pre;
		overflow: hidden;
	}

	.banner-wide {
		display: none;
		font-size: 1.2em;
	}

	.banner-narrow {
		/* 38 columns must fit inside the viewport minus gutters */
		font-size: min(1.2em, calc((100vw - 4rem) / 17.5));
	}

	@media (min-width: 860px) {
		.banner-wide {
			display: block;
		}
		.banner-narrow {
			display: none;
		}
	}

	/* ---------- ls listing ---------- */
	.ls {
		display: grid;
		gap: 0.15rem;
	}

	.ls-row {
		display: grid;
		grid-template-columns: 10.5ch 1fr;
		column-gap: 1.2ch;
		padding: 0.1rem 0;
	}

	.owner {
		display: none;
	}

	.desc {
		grid-column: 1 / -1;
		padding-left: 2ch;
		color: var(--fg);
		opacity: 0.85;
	}

	@media (min-width: 640px) {
		.ls {
			grid-template-columns: max-content max-content max-content 1fr;
			column-gap: 2ch;
			row-gap: 0.3rem;
		}
		.ls-row {
			display: contents;
		}
		.owner {
			display: block;
		}
		.desc {
			grid-column: auto;
			padding-left: 0;
		}
	}

	.perm {
		color: var(--dim);
	}

	.deprecated .perm,
	.tag {
		color: var(--amber);
		text-shadow:
			0 0 2px rgb(255 190 70 / 0.8),
			0 0 10px rgb(255 170 40 / 0.35);
	}

	.deprecated a {
		text-decoration: line-through;
		text-decoration-thickness: 1px;
	}

	.hash {
		color: var(--amber);
		text-shadow:
			0 0 2px rgb(255 190 70 / 0.8),
			0 0 10px rgb(255 170 40 / 0.35);
	}

	.log {
		display: flex;
		flex-wrap: wrap;
		column-gap: 1ch;
	}

	.socials {
		display: grid;
		grid-template-columns: max-content 1fr;
		column-gap: 1ch;
	}

	.socials li {
		display: contents;
	}

	.host {
		display: none;
	}

	@media (min-width: 640px) {
		.host {
			display: inline;
		}
	}

	/* ---------- links ---------- */
	a {
		color: var(--hi);
		text-decoration: underline;
		text-decoration-color: var(--dim);
		text-underline-offset: 3px;
		overflow-wrap: anywhere;
	}

	a:hover {
		background: var(--fg);
		color: var(--bg);
		text-shadow: none;
		text-decoration: none;
		box-shadow: 0 0 12px rgb(120 255 160 / 0.6);
	}

	a:focus-visible {
		outline: 2px dashed var(--amber);
		outline-offset: 3px;
		background: var(--fg);
		color: var(--bg);
		text-shadow: none;
	}

	/* ---------- cursor ---------- */
	.cursor {
		display: inline-block;
		width: 0.6em;
		height: 1.05em;
		background: var(--fg);
		box-shadow: 0 0 10px rgb(120 255 160 / 0.7);
		vertical-align: text-bottom;
		animation: blink 1.06s steps(1) infinite;
	}

	@keyframes blink {
		50% {
			opacity: 0;
		}
	}

	.footnote {
		margin-top: 1rem;
		text-align: center;
		font-size: 0.85em;
		opacity: 0.8;
	}

	/* ---------- boot sequence (content is in the SSR HTML; this only reveals it) ---------- */
	@media (prefers-reduced-motion: no-preference) {
		.block {
			animation: boot-in 1ms both;
			animation-delay: calc(var(--i) * 140ms);
		}

		.block .cmd {
			animation: type 220ms steps(var(--n)) both;
			animation-delay: calc(var(--i) * 140ms);
		}

		.block .out {
			animation: boot-in 1ms both;
			animation-delay: calc(var(--i) * 140ms + 240ms);
		}

		.screen {
			animation: power-on 380ms ease-out both;
		}
	}

	@media (prefers-reduced-motion: reduce) {
		.scanlines,
		.cursor {
			animation: none;
		}
	}

	@keyframes boot-in {
		from {
			visibility: hidden;
		}
		to {
			visibility: visible;
		}
	}

	@keyframes type {
		from {
			max-width: 0;
		}
		to {
			max-width: calc(var(--n) * 1ch + 1ch);
		}
	}

	@keyframes power-on {
		0% {
			opacity: 0;
			transform: scaleY(0.02);
			filter: brightness(3);
		}
		60% {
			opacity: 1;
			transform: scaleY(1.01);
			filter: brightness(1.6);
		}
		100% {
			transform: none;
			filter: none;
		}
	}
</style>
