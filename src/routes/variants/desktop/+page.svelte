<script lang="ts">
	import type { Snippet } from 'svelte';
	import { tick } from 'svelte';
	import { currentRole, projects, openSource, publications, socials } from '$lib/data';

	type WinId = 'about' | 'projects' | 'oss' | 'writing' | 'trash';
	type Win = {
		title: string;
		open: boolean;
		shaded: boolean;
		x: number;
		y: number;
		w: number;
		z: number;
	};

	const active = projects.filter((p) => !p.deprecated);
	const trashed = projects.filter((p) => p.deprecated);

	let wins = $state<Record<WinId, Win>>({
		projects: { title: 'Projects', open: true, shaded: false, x: 520, y: 40, w: 440, z: 2 },
		oss: { title: 'Open Source', open: true, shaded: false, x: 150, y: 410, w: 540, z: 3 },
		writing: { title: 'Writing', open: true, shaded: false, x: 740, y: 480, w: 380, z: 4 },
		trash: { title: 'Trash', open: true, shaded: false, x: 780, y: 690, w: 330, z: 1 },
		about: { title: 'About Torsten', open: true, shaded: false, x: 36, y: 56, w: 430, z: 5 }
	});
	let zTop = $state(5);
	let clock = $state('9:41 AM');
	let day = $state('Wed');
	let deskEl: HTMLElement | undefined = $state();

	const icons: { id: WinId; label: string; kind: 'folder' | 'doc' | 'mac' | 'trash' }[] = [
		{ id: 'about', label: 'About', kind: 'mac' },
		{ id: 'projects', label: 'Projects', kind: 'folder' },
		{ id: 'oss', label: 'Open Source', kind: 'folder' },
		{ id: 'writing', label: 'Writing', kind: 'doc' },
		{ id: 'trash', label: 'Trash', kind: 'trash' }
	];

	let front = $derived(
		(Object.keys(wins) as WinId[])
			.filter((id) => wins[id].open)
			.sort((a, b) => wins[b].z - wins[a].z)[0]
	);

	const isDesktop = () =>
		typeof window !== 'undefined' && window.matchMedia('(min-width: 768px)').matches;

	$effect(() => {
		const update = () => {
			const d = new Date();
			clock = d.toLocaleTimeString('en-US', { hour: 'numeric', minute: '2-digit' });
			day = d.toLocaleDateString('en-US', { weekday: 'short' });
		};
		update();
		const t = setInterval(update, 1000);
		return () => clearInterval(t);
	});

	// Fit the default layout into narrower desktop viewports.
	$effect(() => {
		if (!deskEl) return;
		const max = deskEl.clientWidth - 120;
		for (const id of Object.keys(wins) as WinId[]) {
			const w = wins[id];
			if (w.x + w.w > max) w.x = Math.max(12, Math.round((w.x * max) / 1160));
		}
	});

	function raise(id: WinId) {
		if (wins[id].z === zTop) return;
		zTop += 1;
		wins[id].z = zTop;
	}

	async function openWin(id: WinId) {
		wins[id].open = true;
		wins[id].shaded = false;
		raise(id);
		await tick();
		const el = document.getElementById(`win-${id}`);
		if (!el) return;
		if (!isDesktop()) el.scrollIntoView({ block: 'start' });
		el.focus({ preventScroll: isDesktop() });
	}

	async function closeWin(id: WinId) {
		wins[id].open = false;
		await tick();
		document.getElementById(`icon-${id}`)?.focus();
	}

	let drag: { id: WinId; dx: number; dy: number } | null = $state(null);

	function startDrag(e: PointerEvent, id: WinId) {
		raise(id);
		if (!isDesktop() || e.button !== 0) return;
		if ((e.target as HTMLElement).closest('button')) return;
		(e.currentTarget as HTMLElement).setPointerCapture(e.pointerId);
		drag = { id, dx: e.clientX - wins[id].x, dy: e.clientY - wins[id].y };
		e.preventDefault();
	}

	function moveDrag(e: PointerEvent) {
		if (!drag || !deskEl) return;
		const w = wins[drag.id];
		const maxX = deskEl.clientWidth - 60;
		const maxY = deskEl.clientHeight - 30;
		w.x = Math.min(maxX, Math.max(60 - w.w, e.clientX - drag.dx));
		w.y = Math.min(maxY, Math.max(0, e.clientY - drag.dy));
	}

	function endDrag() {
		drag = null;
	}

	function nudge(e: KeyboardEvent, id: WinId) {
		const step = e.shiftKey ? 40 : 10;
		const d: Record<string, [number, number]> = {
			ArrowLeft: [-step, 0],
			ArrowRight: [step, 0],
			ArrowUp: [0, -step],
			ArrowDown: [0, step]
		};
		if (!d[e.key] || !isDesktop()) return;
		e.preventDefault();
		wins[id].x += d[e.key][0];
		wins[id].y = Math.max(0, wins[id].y + d[e.key][1]);
	}
</script>

<svelte:head>
	<title>Torsten Dittmann — desktop</title>
	<meta name="description" content="Torsten Dittmann — Product Architect from Germany." />
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous" />
	<link
		href="https://fonts.googleapis.com/css2?family=Pixelify+Sans:wght@400;500;600;700&display=swap"
		rel="stylesheet"
	/>
</svelte:head>

<svelte:window onpointerup={endDrag} onpointercancel={endDrag} />

{#snippet icon(kind: 'folder' | 'doc' | 'mac' | 'trash', size = 32)}
	<svg
		width={size}
		height={size}
		viewBox="0 0 32 32"
		shape-rendering="crispEdges"
		aria-hidden="true"
		class="glyph"
	>
		{#if kind === 'folder'}
			<path d="M2 7h11l2 3h15v18H2z" fill="#000" />
			<path d="M3 8h9.5l2 3H29v16H3z" fill="#b7b3ff" />
			<path d="M3 13h26v14H3z" fill="#9e99f5" />
			<path d="M3 12h26v1H3z" fill="#000" />
			<path d="M4 14h24v1H4z" fill="#d6d4ff" />
		{:else if kind === 'doc'}
			<path d="M6 2h14l7 7v21H6z" fill="#000" />
			<path d="M7 3h12v7h7v19H7z" fill="#fff" />
			<path d="M20 4l5 5h-5z" fill="#ccc" />
			<path d="M10 13h13v1H10zM10 16h13v1H10zM10 19h13v1H10zM10 22h9v1h-9z" fill="#555" />
		{:else if kind === 'mac'}
			<path d="M6 1h20v27H6z" fill="#000" />
			<path d="M7 2h18v25H7z" fill="#e4dfd2" />
			<path d="M9 4h14v12H9z" fill="#000" />
			<path d="M10 5h12v10H10z" fill="#5b8fd6" />
			<path d="M13 8h1v2h-1zM18 8h1v2h-1zM13 12h6v1h-6z" fill="#fff" />
			<path d="M9 21h6v1H9zM18 21h5v1h-5z" fill="#777" />
			<path d="M6 28h20v3H6z" fill="#000" />
			<path d="M7 28h18v2H7z" fill="#c9c3b3" />
		{:else}
			<path d="M8 6h16v1H8zM13 3h6v3h-6z" fill="#000" />
			<path d="M14 4h4v2h-4z" fill="#ddd" />
			<path d="M6 7h20v3H6z" fill="#000" />
			<path d="M7 8h18v1H7z" fill="#ddd" />
			<path d="M8 10h16v20H8z" fill="#000" />
			<path d="M9 10h14v19H9z" fill="#e2e2e2" />
			<path d="M12 12h1v15h-1zM16 12h1v15h-1zM20 12h1v15h-1z" fill="#777" />
			<path d="M22 9h3v2h-3zM24 5h2v4h-2z" fill="#f4f0a8" />
		{/if}
	</svg>
{/snippet}

{#snippet win(id: WinId, body: Snippet, status: string)}
	{@const w = wins[id]}
	{#if w.open}
		<section
			id="win-{id}"
			class="win"
			class:inactive={front !== id}
			class:shaded={w.shaded}
			class:dragging={drag?.id === id}
			style="--x:{w.x}px; --y:{w.y}px; --w:{w.w}px; z-index:{w.z}"
			aria-labelledby="title-{id}"
			tabindex="-1"
			onpointerdown={() => raise(id)}
			onfocusin={() => raise(id)}
		>
			<!-- svelte-ignore a11y_no_static_element_interactions -->
			<header
				class="titlebar"
				onpointerdown={(e) => startDrag(e, id)}
				onpointermove={moveDrag}
				onkeydown={(e) => nudge(e, id)}
			>
				<button class="box close" aria-label="Close {w.title}" onclick={() => closeWin(id)}
				></button>
				<span class="stripes" aria-hidden="true"></span>
				<h2 id="title-{id}" class="wtitle">{w.title}</h2>
				<span class="stripes" aria-hidden="true"></span>
				<button
					class="box shade"
					aria-label="{w.shaded ? 'Expand' : 'Collapse'} {w.title}"
					aria-expanded={!w.shaded}
					onclick={() => (w.shaded = !w.shaded)}
				></button>
			</header>
			<div class="winbody" hidden={w.shaded}>
				<div class="info" aria-hidden="true">{status}</div>
				<div class="content">
					{@render body()}
				</div>
				<div class="grow" aria-hidden="true"></div>
			</div>
		</section>
	{/if}
{/snippet}

{#snippet aboutBody()}
	<div class="about">
		<div class="about-head">
			<div class="avatar" aria-hidden="true">{@render icon('mac', 56)}</div>
			<div>
				<h1 class="name">Torsten Dittmann</h1>
				<p class="tagline">Product Architect from Germany.</p>
			</div>
		</div>
		<dl class="specs">
			<div>
				<dt>Current role</dt>
				<dd>{currentRole.role}</dd>
			</div>
			<div>
				<dt>Company</dt>
				<dd>{currentRole.company}</dd>
			</div>
			<div>
				<dt>Location</dt>
				<dd>Germany</dd>
			</div>
		</dl>
		<div class="socials">
			<span class="label">Find me on</span>
			<ul>
				{#each socials as s (s.name)}
					<li>
						<a class="pbtn" href={s.url} target="_blank" rel="noopener noreferrer">{s.name}</a>
					</li>
				{/each}
			</ul>
		</div>
	</div>
{/snippet}

{#snippet listBody(
	items: { name: string; description: string; url: string }[],
	kind: 'doc' | 'folder'
)}
	<div class="colhead" aria-hidden="true">
		<span>Name</span><span class="hidden sm:inline">Kind</span>
	</div>
	<ul class="list">
		{#each items as item (item.name)}
			<li>
				<span class="li-icon">{@render icon(kind, 20)}</span>
				<div class="li-text">
					<a class="li-name" href={item.url} target="_blank" rel="noopener noreferrer"
						>{item.name}</a
					>
					<p class="li-desc">{item.description}</p>
				</div>
				<span class="li-kind hidden sm:inline">{kind === 'doc' ? 'application' : 'repository'}</span
				>
			</li>
		{/each}
	</ul>
{/snippet}

{#snippet projectsBody()}
	{@render listBody(active, 'doc')}
{/snippet}

{#snippet ossBody()}
	{@render listBody(openSource, 'folder')}
{/snippet}

{#snippet writingBody()}
	<div class="doc">
		<p class="doc-meta">SimpleText — Publications</p>
		{#each publications as pub (pub.url)}
			<article class="pub">
				<a class="pub-title" href={pub.url} target="_blank" rel="noopener noreferrer">{pub.title}</a
				>
				<p class="pub-src">{pub.publication} · {pub.year}</p>
			</article>
		{/each}
	</div>
{/snippet}

{#snippet trashBody()}
	<ul class="list">
		{#each trashed as item (item.name)}
			<li>
				<span class="li-icon">{@render icon('doc', 20)}</span>
				<div class="li-text">
					<a class="li-name" href={item.url} target="_blank" rel="noopener noreferrer"
						>{item.name}</a
					>
					<span class="dep">Deprecated</span>
					<p class="li-desc">{item.description}</p>
				</div>
			</li>
		{/each}
	</ul>
	<p class="trash-note">Retired, but not forgotten. Nobody has emptied the trash.</p>
{/snippet}

<div class="os min-h-screen">
	<nav class="menubar" aria-label="Menu bar">
		<div class="menus">
			<span class="logo" aria-hidden="true">
				<svg width="16" height="16" viewBox="0 0 16 16" shape-rendering="crispEdges">
					<path d="M2 2h12v3H9v9H7V5H2z" fill="#000" />
					<path d="M3 3h10v1H8v9H8V4H3z" fill="#4a45c9" />
				</svg>
			</span>
			<span class="menu strong">Finder</span>
			<span class="menu hidden sm:inline">File</span>
			<span class="menu hidden sm:inline">Edit</span>
			<span class="menu hidden sm:inline">View</span>
			<span class="menu hidden md:inline">Special</span>
			<span class="menu hidden md:inline">Help</span>
		</div>
		<div class="menus">
			<span class="menu clock"><time>{day} {clock}</time></span>
			<span class="menu app" aria-hidden="true">
				<svg width="14" height="14" viewBox="0 0 16 16" shape-rendering="crispEdges">
					<path d="M1 1h14v14H1z" fill="#000" />
					<path d="M2 2h12v12H2z" fill="#9e99f5" />
					<path d="M8 2h6v12H8z" fill="#5b8fd6" />
					<path d="M5 5h1v2H5zM10 5h1v2h-1zM5 10h6v1H5z" fill="#000" />
				</svg>
				<span class="hidden sm:inline">Finder</span>
			</span>
		</div>
	</nav>

	<main class="desk" bind:this={deskEl}>
		<ul class="icons" aria-label="Desktop">
			{#each icons as ic (ic.id)}
				<li>
					<button
						id="icon-{ic.id}"
						class="dicon"
						class:selected={wins[ic.id].open && front === ic.id}
						onclick={() => openWin(ic.id)}
						aria-controls="win-{ic.id}"
						aria-label="Open {ic.label}"
					>
						<span class="dicon-img">{@render icon(ic.kind, 40)}</span>
						<span class="dicon-label">{ic.label}</span>
					</button>
				</li>
			{/each}
		</ul>

		<div class="windows">
			{@render win('about', aboutBody, 'Product Architect · Germany')}
			{@render win('projects', projectsBody, `${active.length} items`)}
			{@render win('oss', ossBody, `${openSource.length} items`)}
			{@render win('writing', writingBody, `${publications.length} document`)}
			{@render win('trash', trashBody, `${trashed.length} item`)}
			<p class="hint">Tip: drag windows by their title bar · click icons to reopen</p>
			{#if !Object.values(wins).some((w) => w.open)}
				<p class="empty">All windows closed. Click an icon to open one again.</p>
			{/if}
		</div>
	</main>
</div>

<style>
	.os {
		--ink: #111;
		--plat: #dddddd;
		--hi: #ffffff;
		--lo: #8a8a8a;
		--sel: #3b37b8;
		font-family: Geneva, Verdana, 'DejaVu Sans', system-ui, sans-serif;
		color: var(--ink);
		background-color: #6566a8;
		background-image:
			conic-gradient(#6f70b3 25%, transparent 0 50%, #6f70b3 0 75%, transparent 0),
			radial-gradient(ellipse at 30% 20%, rgba(255, 255, 255, 0.08), transparent 60%);
		background-size:
			4px 4px,
			100% 100%;
		overflow-x: hidden;
		-webkit-font-smoothing: auto;
	}
	.os :where(h1, h2, .menu, .wtitle, .dicon-label, .info, .colhead, .label, .pbtn, dt, .dep) {
		font-family: 'Pixelify Sans', Geneva, Verdana, sans-serif;
	}

	/* ---------- Menu bar ---------- */
	.menubar {
		position: sticky;
		top: 0;
		z-index: 1000;
		display: flex;
		justify-content: space-between;
		align-items: center;
		height: 26px;
		padding: 0 10px;
		background: linear-gradient(#f6f6f6, #dcdcdc);
		border-bottom: 1px solid #000;
		box-shadow: inset 0 -1px 0 #bbb;
		font-size: 15px;
	}
	.menus {
		display: flex;
		align-items: center;
		gap: 2px;
		height: 100%;
	}
	.menu {
		padding: 0 10px;
		line-height: 26px;
		white-space: nowrap;
		cursor: default;
	}
	.menu.strong {
		font-weight: 700;
	}
	.logo {
		display: grid;
		place-items: center;
		padding: 0 10px 0 4px;
	}
	.app {
		display: flex;
		align-items: center;
		gap: 6px;
		border-left: 1px solid #aaa;
		box-shadow: -1px 0 0 #fff;
	}
	.clock {
		font-variant-numeric: tabular-nums;
	}

	/* ---------- Desktop ---------- */
	.desk {
		position: relative;
		padding: 16px;
	}
	.icons {
		list-style: none;
		margin: 0 0 18px;
		padding: 0;
		display: grid;
		grid-template-columns: repeat(5, minmax(0, 1fr));
		gap: 4px;
	}
	.dicon {
		display: flex;
		flex-direction: column;
		align-items: center;
		gap: 4px;
		width: 100%;
		padding: 4px 2px;
		background: none;
		border: 0;
		cursor: pointer;
		color: #fff;
	}
	.dicon-img {
		display: grid;
		place-items: center;
		padding: 2px;
	}
	.dicon-label {
		font-size: 13px;
		line-height: 1.15;
		padding: 1px 4px;
		background: #fff;
		color: #000;
		text-align: center;
	}
	.dicon.selected .dicon-img {
		filter: brightness(0.55) sepia(1) hue-rotate(200deg) saturate(3);
	}
	.dicon.selected .dicon-label,
	.dicon:hover .dicon-label {
		background: var(--sel);
		color: #fff;
	}
	.dicon:focus-visible {
		outline: none;
	}
	.dicon:focus-visible .dicon-label {
		outline: 2px dashed #fff;
		outline-offset: 2px;
		background: var(--sel);
		color: #fff;
	}

	.windows {
		display: flex;
		flex-direction: column;
		gap: 16px;
	}
	.hint {
		display: none;
	}
	.empty {
		color: #fff;
		font-size: 14px;
		text-align: center;
		padding: 40px 0;
	}

	/* ---------- Windows (Platinum) ---------- */
	.win {
		position: relative;
		background: var(--plat);
		border: 1px solid #000;
		box-shadow:
			inset 1px 1px 0 var(--hi),
			inset -1px -1px 0 var(--lo),
			2px 2px 0 rgba(0, 0, 0, 0.45);
		padding: 0 4px 4px;
		outline: none;
	}
	.win:focus-visible {
		outline: 2px dashed #fff;
		outline-offset: 3px;
	}
	.titlebar {
		display: flex;
		align-items: center;
		gap: 6px;
		height: 24px;
		padding: 0 2px;
		touch-action: none;
		user-select: none;
	}
	.stripes {
		flex: 1;
		height: 12px;
		background: repeating-linear-gradient(
			to bottom,
			#fff 0 1px,
			#8a8a8a 1px 2px,
			transparent 2px 3px
		);
		opacity: 1;
	}
	.wtitle {
		margin: 0;
		font-size: 15px;
		font-weight: 600;
		white-space: nowrap;
		padding: 0 4px;
		line-height: 1;
	}
	.box {
		flex: none;
		width: 14px;
		height: 14px;
		padding: 0;
		border: 1px solid #000;
		background: linear-gradient(135deg, #bbb, #fff 55%);
		box-shadow:
			inset 1px 1px 0 #fff,
			inset -1px -1px 0 #888;
		cursor: pointer;
		position: relative;
	}
	.box:active {
		background: #888;
		box-shadow: inset 1px 1px 0 #555;
	}
	.box.shade::after {
		content: '';
		position: absolute;
		left: 1px;
		right: 1px;
		top: 5px;
		height: 3px;
		border-top: 1px solid #000;
		border-bottom: 1px solid #000;
	}
	.box:focus-visible {
		outline: 2px solid var(--sel);
		outline-offset: 1px;
	}
	.inactive .stripes {
		visibility: hidden;
	}
	.inactive .box {
		background: var(--plat);
		border-color: #999;
		box-shadow: none;
	}
	.inactive .box.shade::after {
		border-color: #999;
	}
	.inactive .wtitle {
		color: #5f5f5f;
	}
	.inactive {
		box-shadow:
			inset 1px 1px 0 var(--hi),
			inset -1px -1px 0 var(--lo);
	}
	.shaded {
		padding-bottom: 0;
	}

	.winbody {
		border: 1px solid #000;
		border-color: #777 #fff #fff #777;
		position: relative;
	}
	.info {
		display: flex;
		justify-content: center;
		font-size: 13px;
		padding: 3px 6px;
		border-bottom: 1px solid #000;
		background: var(--plat);
		color: #333;
	}
	.content {
		background: #fff;
		border-left: 1px solid #000;
		border-right: 1px solid #000;
		border-bottom: 1px solid #000;
		font-size: 14px;
		line-height: 1.45;
	}
	.grow {
		position: absolute;
		right: 1px;
		bottom: 1px;
		width: 12px;
		height: 12px;
		background:
			linear-gradient(135deg, transparent 45%, #888 45% 55%, transparent 55%),
			linear-gradient(135deg, transparent 70%, #888 70% 80%, transparent 80%);
		display: none;
	}

	.content a {
		color: #1f1ba8;
		text-decoration: underline;
		text-underline-offset: 2px;
	}
	.content a:hover {
		background: var(--sel);
		color: #fff;
		text-decoration: none;
	}
	.content a:focus-visible {
		outline: 2px solid var(--sel);
		outline-offset: 2px;
	}

	/* About */
	.about {
		padding: 16px;
	}
	.about-head {
		display: flex;
		gap: 14px;
		align-items: center;
		padding-bottom: 14px;
		border-bottom: 1px solid #ccc;
	}
	.avatar {
		flex: none;
		width: 72px;
		height: 72px;
		display: grid;
		place-items: center;
		background: var(--plat);
		border: 1px solid #000;
		box-shadow:
			inset 1px 1px 0 #fff,
			inset -1px -1px 0 #888;
	}
	.name {
		margin: 0;
		font-size: 28px;
		font-weight: 700;
		line-height: 1;
		letter-spacing: -0.01em;
	}
	.tagline {
		margin: 6px 0 0;
		font-size: 15px;
		color: #333;
	}
	.specs {
		margin: 12px 0;
		display: grid;
		gap: 6px;
	}
	.specs div {
		display: grid;
		grid-template-columns: 7.5rem 1fr;
		gap: 8px;
		align-items: baseline;
	}
	.specs dt {
		font-size: 14px;
		color: #555;
		text-align: right;
	}
	.specs dd {
		margin: 0;
		font-weight: 700;
	}
	.socials {
		display: flex;
		flex-wrap: wrap;
		align-items: center;
		gap: 10px;
		padding-top: 12px;
		border-top: 1px solid #ccc;
	}
	.label {
		font-size: 14px;
		color: #555;
	}
	.socials ul {
		list-style: none;
		margin: 0;
		padding: 0;
		display: flex;
		flex-wrap: wrap;
		gap: 8px;
	}
	.content a.pbtn {
		display: inline-block;
		padding: 3px 14px;
		font-size: 14px;
		color: #000;
		text-decoration: none;
		background: linear-gradient(#fff, #d4d4d4);
		border: 1px solid #000;
		border-radius: 5px;
		box-shadow:
			inset 1px 1px 0 #fff,
			inset -1px -1px 0 #999;
	}
	.content a.pbtn:hover {
		background: linear-gradient(#e6e6ff, #c3c1f2);
		color: #000;
	}
	.content a.pbtn:active {
		background: #777;
		color: #fff;
	}

	/* Finder list */
	.colhead {
		display: flex;
		justify-content: space-between;
		font-size: 13px;
		padding: 2px 10px 2px 42px;
		background: linear-gradient(#f4f4f4, #dedede);
		border-bottom: 1px solid #999;
		color: #333;
	}
	.list {
		list-style: none;
		margin: 0;
		padding: 4px 0;
	}
	.list li {
		display: flex;
		gap: 10px;
		align-items: flex-start;
		padding: 7px 10px;
	}
	.list li:nth-child(even) {
		background: #f1f1f8;
	}
	.li-icon {
		flex: none;
		padding-top: 1px;
	}
	.li-text {
		flex: 1;
		min-width: 0;
	}
	.content .li-name {
		font-weight: 700;
		overflow-wrap: anywhere;
	}
	.li-desc {
		margin: 2px 0 0;
		color: #333;
		font-size: 13px;
	}
	.li-kind {
		flex: none;
		color: #666;
		font-size: 12px;
		padding-top: 2px;
	}
	.dep {
		display: inline-block;
		margin-left: 6px;
		padding: 0 5px;
		font-size: 12px;
		background: #000;
		color: #fff;
	}
	.trash-note {
		margin: 0;
		padding: 6px 10px 10px;
		font-size: 12px;
		color: #666;
		font-style: italic;
	}

	/* Writing */
	.doc {
		padding: 14px 16px 18px;
		background: repeating-linear-gradient(to bottom, transparent 0 23px, #e5e9f5 23px 24px) #fff;
	}
	.doc-meta {
		margin: 0 0 10px;
		font-size: 12px;
		color: #666;
		text-transform: uppercase;
		letter-spacing: 0.04em;
	}
	.content .pub-title {
		font-size: 16px;
		font-weight: 700;
		line-height: 1.35;
	}
	.pub-src {
		margin: 4px 0 0;
		color: #444;
		font-size: 13px;
	}

	/* ---------- Desktop layout ---------- */
	@media (min-width: 768px) {
		.desk {
			padding: 0;
			min-height: max(calc(100vh - 26px), 940px);
		}
		.icons {
			position: absolute;
			top: 16px;
			right: 14px;
			width: 96px;
			grid-template-columns: 1fr;
			gap: 14px;
			margin: 0;
			z-index: 0;
		}
		.windows {
			display: block;
		}
		.win {
			position: absolute;
			left: var(--x);
			top: var(--y);
			width: var(--w);
			max-width: calc(100vw - 150px);
		}
		.titlebar {
			cursor: grab;
		}
		.dragging .titlebar {
			cursor: grabbing;
		}
		.dragging {
			opacity: 0.92;
		}
		.grow {
			display: block;
		}
		.hint {
			display: block;
			position: absolute;
			left: 16px;
			bottom: 12px;
			margin: 0;
			font-family: 'Pixelify Sans', sans-serif;
			font-size: 13px;
			color: #fff;
			text-shadow: 1px 1px 0 #2d2d6b;
		}
		.empty {
			position: absolute;
			left: 0;
			right: 120px;
			top: 40%;
		}
	}

	@media (max-width: 767px) {
		.win {
			box-shadow:
				inset 1px 1px 0 var(--hi),
				inset -1px -1px 0 var(--lo),
				3px 3px 0 rgba(0, 0, 0, 0.45);
		}
		.inactive .stripes {
			visibility: visible;
		}
		.inactive .wtitle {
			color: inherit;
		}
		.name {
			font-size: 24px;
		}
		.specs div {
			grid-template-columns: 6.5rem 1fr;
		}
	}

	@media (prefers-reduced-motion: no-preference) {
		.win {
			animation: zoomin 160ms steps(4, end);
		}
		@keyframes zoomin {
			from {
				transform: scale(0.85);
				opacity: 0;
			}
		}
	}
</style>
