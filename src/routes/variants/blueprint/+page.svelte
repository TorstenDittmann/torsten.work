<script lang="ts">
	import { currentRole, projects, openSource, publications, socials } from '$lib/data';

	const pad = (n: number) => String(n).padStart(2, '0');
	const zonesX = ['A', 'B', 'C', 'D', 'E', 'F'];
	const zonesY = ['1', '2', '3', '4', '5'];

	let active = $state<string | null>(null);

	const host = (url: string) => url.replace(/^https?:\/\/(www\.)?/, '').replace(/\/$/, '');
</script>

<svelte:head>
	<title>Torsten Dittmann — blueprint</title>
	<link rel="preconnect" href="https://fonts.googleapis.com" />
	<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin="anonymous" />
	<link
		href="https://fonts.googleapis.com/css2?family=Architects+Daughter&family=IBM+Plex+Mono:ital,wght@0,400;0,500;0,600;1,400&display=swap"
		rel="stylesheet"
	/>
</svelte:head>

{#snippet dim(label: string, cls = '')}
	<div class="dim {cls}" aria-hidden="true">
		<span class="dim-tick"></span>
		<svg class="dim-arrow" viewBox="0 0 10 10" preserveAspectRatio="none"
			><path d="M0 5 L10 1 L10 9 Z" /></svg
		>
		<span class="dim-line"></span>
		<span class="dim-label">{label}</span>
		<span class="dim-line"></span>
		<svg class="dim-arrow" viewBox="0 0 10 10" preserveAspectRatio="none"
			><path d="M10 5 L0 1 L0 9 Z" /></svg
		>
		<span class="dim-tick"></span>
	</div>
{/snippet}

{#snippet corners()}
	<span class="reg tl" aria-hidden="true"></span>
	<span class="reg tr" aria-hidden="true"></span>
	<span class="reg bl" aria-hidden="true"></span>
	<span class="reg br" aria-hidden="true"></span>
{/snippet}

{#snippet sectionHead(num: string, title: string, note: string)}
	<div class="sec-head">
		<span class="sec-num">{num}</span>
		<h2 class="sec-title">{title}</h2>
		<span class="sec-rule" aria-hidden="true"></span>
		<span class="hand sec-note">{note}</span>
	</div>
{/snippet}

<div class="bp min-h-screen">
	<div class="sheet">
		<!-- zone markers -->
		<div class="zones-x" aria-hidden="true">
			{#each zonesX as z (z)}<span>{z}</span>{/each}
		</div>
		<div class="zones-y" aria-hidden="true">
			{#each zonesY as z (z)}<span>{z}</span>{/each}
		</div>

		<div class="inner">
			<!-- HEADER -->
			<header class="hdr">
				<div class="hdr-meta">
					<span>DWG NO. TD-2026-001</span>
					<span class="hide-sm">·</span>
					<span>ELEVATION VIEW — FRONT</span>
				</div>

				<div class="name-wrap">
					<svg class="crosshair ch-name" viewBox="0 0 40 40" aria-hidden="true">
						<circle cx="20" cy="20" r="9" />
						<path d="M20 0 V40 M0 20 H40" />
					</svg>
					<h1 class="name">Torsten Dittmann</h1>
					{@render dim('L = 1 HUMAN · SCALE 1:1', 'dim-name')}
				</div>

				<div class="tagline-row">
					<p class="tagline">Product Architect from Germany.</p>
					<span class="hand callout-note">
						<svg class="leader-svg" viewBox="0 0 60 24" aria-hidden="true">
							<path d="M58 20 L20 20 L2 4" />
							<circle cx="2" cy="4" r="2" />
						</svg>
						designs products, not just buildings
					</span>
				</div>

				<nav class="socials" aria-label="Social links">
					<span class="lbl">CONNECTION POINTS</span>
					<ul>
						{#each socials as s, i (s.name)}
							<li>
								<a href={s.url} target="_blank" rel="noopener noreferrer" class="link sock">
									<span class="sock-id">C{i + 1}</span>
									{s.name}
									<span aria-hidden="true">↗</span>
								</a>
							</li>
						{/each}
					</ul>
				</nav>
			</header>

			<!-- CURRENT ROLE -->
			<section class="sec" aria-labelledby="role-h">
				<div class="sec-head">
					<span class="sec-num">01</span>
					<h2 class="sec-title" id="role-h">Current Assignment</h2>
					<span class="sec-rule" aria-hidden="true"></span>
					<span class="hand sec-note">see spec below</span>
				</div>
				<div class="role box">
					{@render corners()}
					<div class="role-cell">
						<span class="lbl">ROLE</span>
						<span class="role-val">{currentRole.role}</span>
					</div>
					<div class="role-cell">
						<span class="lbl">ORGANISATION</span>
						<span class="role-val">{currentRole.company}</span>
					</div>
					<div class="role-cell">
						<span class="lbl">STATUS</span>
						<span class="role-val status"
							><span class="dot" aria-hidden="true"></span>IN SERVICE</span
						>
					</div>
				</div>
			</section>

			<!-- PROJECTS -->
			<section class="sec" aria-labelledby="proj-h">
				<div class="sec-head">
					<span class="sec-num">02</span>
					<h2 class="sec-title" id="proj-h">Components — Projects</h2>
					<span class="sec-rule" aria-hidden="true"></span>
					<span class="hand sec-note">{projects.length} parts, assembled by hand</span>
				</div>

				<ul class="comp-grid">
					{#each projects as p, i (p.name)}
						{@const id = `P-${pad(i + 1)}`}
						<li class="comp-li">
							<a
								href={p.url}
								target="_blank"
								rel="noopener noreferrer"
								class="comp box"
								class:obsolete={p.deprecated}
								class:on={active === id}
								onmouseenter={() => (active = id)}
								onmouseleave={() => (active = null)}
								onfocus={() => (active = id)}
								onblur={() => (active = null)}
							>
								{@render corners()}
								<div class="comp-top">
									<span class="part">{id}</span>
									<span class="comp-host">{host(p.url)}</span>
								</div>
								<h3 class="comp-name">{p.name}</h3>
								<p class="comp-desc">{p.description}</p>
								<div class="comp-foot">
									{#if p.deprecated}
										<span class="stamp">OBSOLETE · SUPERSEDED</span>
									{:else}
										<span class="lbl">REV A · ACTIVE</span>
									{/if}
									<span class="go" aria-hidden="true">→</span>
								</div>
							</a>
							<span class="hand comp-callout" aria-hidden="true">
								<svg viewBox="0 0 40 20"
									><path d="M2 18 L14 18 L38 2" /><circle cx="38" cy="2" r="2" /></svg
								>
								{p.deprecated
									? 'retired — kept for reference'
									: i === 0
										? 'latest build'
										: 'in production'}
							</span>
						</li>
					{/each}
					<li class="comp-li legend-li" aria-hidden="true">
						<div class="legend">
							<span class="lbl">LEGEND</span>
							<div class="lg-row"><span class="lg-sw solid"></span>Active component</div>
							<div class="lg-row"><span class="lg-sw dashed"></span>Obsolete / superseded</div>
							<div class="lg-row"><span class="lg-sw hot"></span>Selected (hover / focus)</div>
							<svg class="legend-ch crosshair" viewBox="0 0 40 40">
								<circle cx="20" cy="20" r="9" />
								<path d="M20 0 V40 M0 20 H40" />
							</svg>
						</div>
					</li>
				</ul>
			</section>

			<!-- OPEN SOURCE BOM -->
			<section class="sec" aria-labelledby="oss-h">
				<div class="sec-head">
					<span class="sec-num">03</span>
					<h2 class="sec-title" id="oss-h">Bill of Materials — Open Source</h2>
					<span class="sec-rule" aria-hidden="true"></span>
					<span class="hand sec-note">all parts freely available</span>
				</div>

				<div class="bom-wrap box">
					{@render corners()}
					<table class="bom">
						<thead>
							<tr>
								<th scope="col" class="c-item">ITEM</th>
								<th scope="col" class="c-part">PART NO.</th>
								<th scope="col">DESIGNATION</th>
								<th scope="col" class="c-desc">DESCRIPTION</th>
								<th scope="col" class="c-qty">QTY</th>
							</tr>
						</thead>
						<tbody>
							{#each openSource as o, i (o.name)}
								<tr>
									<td class="c-item">{pad(i + 1)}</td>
									<td class="c-part">OSS-{pad(i + 1)}</td>
									<td>
										<a href={o.url} target="_blank" rel="noopener noreferrer" class="link bom-link"
											>{o.name}</a
										>
										<span class="bom-desc-sm">{o.description}</span>
									</td>
									<td class="c-desc">{o.description}</td>
									<td class="c-qty">1</td>
								</tr>
							{/each}
						</tbody>
					</table>
				</div>
			</section>

			<!-- WRITING + TITLE BLOCK -->
			<div class="bottom">
				<section class="sec writing" aria-labelledby="pub-h">
					<div class="sec-head">
						<span class="sec-num">04</span>
						<h2 class="sec-title" id="pub-h">Reference Documents</h2>
						<span class="sec-rule" aria-hidden="true"></span>
					</div>
					<ol class="refs">
						{#each publications as pub, i (pub.url)}
							<li>
								<span class="ref-id">REF {pad(i + 1)}</span>
								<a href={pub.url} target="_blank" rel="noopener noreferrer" class="link ref-title"
									>{pub.title}</a
								>
								<span class="ref-meta">{pub.publication} · {pub.year}</span>
							</li>
						{/each}
					</ol>
					<p class="hand notes">
						Notes: 1. All dimensions in lines of code. 2. Do not scale drawing. 3. Built with care
						in Germany.
					</p>
				</section>

				<aside class="tblock" aria-label="Drawing title block">
					<div class="tb-cell tb-title">
						<span class="lbl">TITLE</span>
						<span class="tb-big">PORTFOLIO</span>
					</div>
					<div class="tb-cell tb-wide">
						<span class="lbl">DRAWN BY</span>
						<span class="tb-val">T. DITTMANN</span>
					</div>
					<div class="tb-cell">
						<span class="lbl">SHEET</span>
						<span class="tb-val">1 OF 1</span>
					</div>
					<div class="tb-cell">
						<span class="lbl">REV</span>
						<span class="tb-val">2026</span>
					</div>
					<div class="tb-cell">
						<span class="lbl">SCALE</span>
						<span class="tb-val">1:1</span>
					</div>
					<div class="tb-cell tb-dept">
						<span class="lbl">DEPT</span>
						<span class="tb-val">PRODUCT ARCH.</span>
					</div>
					<div class="tb-cell tb-appr">
						<span class="lbl">APPROVED</span>
						<span class="hand tb-sig">T. Dittmann</span>
					</div>
				</aside>
			</div>
		</div>
	</div>
</div>

<style>
	.bp {
		--ink: #eaf6ff;
		--ink-2: #b4dcf7;
		--ink-3: #7fb3d9;
		--hot: #7ff4ff;
		--paper: #0d3a72;
		--paper-2: #0a3166;
		--line: rgba(200, 230, 255, 0.55);
		--mono: 'IBM Plex Mono', ui-monospace, monospace;
		--hand: 'Architects Daughter', 'Comic Sans MS', cursive;

		color: var(--ink);
		font-family: var(--mono);
		background-color: var(--paper);
		background-image:
			radial-gradient(ellipse at 30% 10%, rgba(255, 255, 255, 0.07), transparent 60%),
			radial-gradient(ellipse at 90% 100%, rgba(0, 10, 40, 0.35), transparent 60%),
			linear-gradient(rgba(190, 225, 255, 0.16) 1px, transparent 1px),
			linear-gradient(90deg, rgba(190, 225, 255, 0.16) 1px, transparent 1px),
			linear-gradient(rgba(190, 225, 255, 0.06) 1px, transparent 1px),
			linear-gradient(90deg, rgba(190, 225, 255, 0.06) 1px, transparent 1px);
		background-size:
			100% 100%,
			100% 100%,
			100px 100px,
			100px 100px,
			20px 20px,
			20px 20px;
		background-position: -1px -1px;
		padding: 16px;
		overflow-x: hidden;
		-webkit-font-smoothing: antialiased;
	}

	.hand {
		font-family: var(--hand);
		color: var(--ink-2);
	}

	.lbl {
		font-size: 10px;
		letter-spacing: 0.16em;
		color: var(--ink-3);
		text-transform: uppercase;
	}

	/* ---------- sheet frame ---------- */
	.sheet {
		position: relative;
		max-width: 1180px;
		margin: 0 auto;
		border: 2px solid var(--ink);
		outline: 1px solid var(--line);
		outline-offset: 5px;
	}
	.inner {
		padding: 24px 18px 20px;
	}
	.zones-x,
	.zones-y {
		display: none;
	}

	/* ---------- links ---------- */
	.link {
		color: var(--ink);
		text-decoration: none;
		transition:
			color 0.15s,
			border-color 0.15s;
	}
	.link:hover {
		color: var(--hot);
	}
	a:focus-visible {
		outline: 2px dashed var(--hot);
		outline-offset: 4px;
	}

	/* ---------- header ---------- */
	.hdr {
		position: relative;
		padding-bottom: 8px;
	}
	.hdr-meta {
		display: flex;
		flex-wrap: wrap;
		gap: 4px 10px;
		font-size: 10px;
		letter-spacing: 0.18em;
		color: var(--ink-3);
	}
	.name-wrap {
		position: relative;
		display: inline-block;
		margin-top: 28px;
		max-width: 100%;
	}
	.name {
		font-family: var(--mono);
		font-weight: 500;
		font-size: clamp(2.1rem, 9vw, 5.25rem);
		line-height: 1;
		letter-spacing: -0.03em;
		text-transform: uppercase;
		color: var(--ink);
		margin: 0;
	}
	.crosshair {
		fill: none;
		stroke: var(--ink-2);
		stroke-width: 1;
	}
	.ch-name {
		position: absolute;
		width: 28px;
		height: 28px;
		left: -14px;
		top: -18px;
		opacity: 0.8;
	}

	/* dimension line */
	.dim {
		display: flex;
		align-items: center;
		gap: 0;
		color: var(--ink-2);
		margin-top: 10px;
		height: 16px;
	}
	.dim-tick {
		width: 1px;
		height: 16px;
		background: var(--ink-2);
	}
	.dim-arrow {
		width: 9px;
		height: 7px;
		fill: var(--ink-2);
		flex: none;
	}
	.dim-line {
		flex: 1;
		height: 1px;
		background: var(--ink-2);
	}
	.dim-label {
		font-size: 10px;
		letter-spacing: 0.14em;
		padding: 0 10px;
		white-space: nowrap;
	}

	.tagline-row {
		display: flex;
		flex-wrap: wrap;
		align-items: center;
		gap: 8px 18px;
		margin-top: 22px;
	}
	.tagline {
		margin: 0;
		font-size: clamp(1.05rem, 2.4vw, 1.35rem);
		color: var(--ink);
	}
	.callout-note {
		display: inline-flex;
		align-items: flex-end;
		gap: 6px;
		font-size: 15px;
	}
	.leader-svg {
		width: 48px;
		height: 20px;
		fill: none;
		stroke: var(--ink-2);
		stroke-width: 1;
		transform: scaleX(-1);
	}
	.leader-svg circle {
		fill: var(--ink-2);
	}

	.socials {
		margin-top: 26px;
		display: flex;
		flex-wrap: wrap;
		align-items: center;
		gap: 10px 16px;
	}
	.socials ul {
		display: flex;
		flex-wrap: wrap;
		gap: 10px;
		list-style: none;
		margin: 0;
		padding: 0;
	}
	.sock {
		display: inline-flex;
		align-items: center;
		gap: 8px;
		border: 1px solid var(--line);
		padding: 6px 12px 6px 6px;
		font-size: 13px;
		background: rgba(10, 40, 90, 0.5);
	}
	.sock:hover {
		border-color: var(--hot);
		box-shadow: 0 0 0 1px var(--hot) inset;
	}
	.sock-id {
		font-size: 10px;
		border: 1px solid var(--line);
		border-radius: 999px;
		width: 22px;
		height: 22px;
		display: grid;
		place-items: center;
		color: var(--ink-2);
	}

	/* ---------- sections ---------- */
	.sec {
		margin-top: 48px;
	}
	.sec-head {
		display: flex;
		align-items: center;
		gap: 12px;
		margin-bottom: 18px;
		flex-wrap: wrap;
	}
	.sec-num {
		font-size: 11px;
		width: 30px;
		height: 30px;
		display: grid;
		place-items: center;
		border: 1px solid var(--ink);
		border-radius: 50%;
		flex: none;
	}
	.sec-title {
		margin: 0;
		font-size: 13px;
		font-weight: 600;
		letter-spacing: 0.2em;
		text-transform: uppercase;
	}
	.sec-rule {
		flex: 1;
		min-width: 24px;
		height: 1px;
		background: repeating-linear-gradient(90deg, var(--line) 0 10px, transparent 10px 14px);
	}
	.sec-note {
		font-size: 15px;
		display: none;
	}

	/* box w/ registration marks */
	.box {
		position: relative;
		border: 1px solid var(--line);
	}
	.reg {
		position: absolute;
		width: 12px;
		height: 12px;
		border-color: var(--ink);
		border-style: solid;
		border-width: 0;
		transition: border-color 0.15s;
		pointer-events: none;
	}
	.reg.tl {
		top: -5px;
		left: -5px;
		border-top-width: 2px;
		border-left-width: 2px;
	}
	.reg.tr {
		top: -5px;
		right: -5px;
		border-top-width: 2px;
		border-right-width: 2px;
	}
	.reg.bl {
		bottom: -5px;
		left: -5px;
		border-bottom-width: 2px;
		border-left-width: 2px;
	}
	.reg.br {
		bottom: -5px;
		right: -5px;
		border-bottom-width: 2px;
		border-right-width: 2px;
	}

	/* role */
	.role {
		display: grid;
		grid-template-columns: 1fr;
		background: rgba(8, 36, 84, 0.45);
	}
	.role-cell {
		display: flex;
		flex-direction: column;
		gap: 6px;
		padding: 14px 16px;
	}
	.role-cell + .role-cell {
		border-top: 1px solid var(--line);
	}
	.role-val {
		font-size: 18px;
		font-weight: 500;
	}
	.status {
		display: inline-flex;
		align-items: center;
		gap: 8px;
		font-size: 14px;
		letter-spacing: 0.14em;
		color: var(--hot);
	}
	.dot {
		width: 8px;
		height: 8px;
		border-radius: 50%;
		background: var(--hot);
		box-shadow: 0 0 10px var(--hot);
	}

	/* components */
	.comp-grid {
		list-style: none;
		margin: 0;
		padding: 0;
		display: grid;
		grid-template-columns: 1fr;
		gap: 26px;
	}
	.comp-li {
		position: relative;
	}
	.comp {
		display: flex;
		flex-direction: column;
		height: 100%;
		padding: 16px 18px 14px;
		color: var(--ink);
		text-decoration: none;
		background: rgba(8, 36, 84, 0.45);
		transition:
			border-color 0.18s,
			background-color 0.18s,
			box-shadow 0.18s;
	}
	.comp.on,
	.comp:hover {
		border-color: var(--hot);
		background: rgba(40, 150, 220, 0.16);
		box-shadow:
			0 0 0 1px var(--hot) inset,
			0 0 24px rgba(127, 244, 255, 0.18);
	}
	.comp.on .reg,
	.comp:hover .reg {
		border-color: var(--hot);
	}
	.comp.on .comp-name,
	.comp:hover .comp-name,
	.comp.on .part,
	.comp:hover .part {
		color: var(--hot);
	}
	.comp.obsolete {
		border-style: dashed;
	}
	.comp.obsolete .comp-name {
		text-decoration: line-through;
		text-decoration-thickness: 1px;
		text-decoration-color: var(--ink-3);
	}
	.comp-top {
		display: flex;
		justify-content: space-between;
		align-items: center;
		gap: 10px;
	}
	.part {
		flex: none;
		white-space: nowrap;
		font-size: 11px;
		font-weight: 600;
		letter-spacing: 0.12em;
		padding: 2px 7px;
		border: 1px solid currentColor;
		transition: color 0.18s;
	}
	.comp-host {
		font-size: 10px;
		color: var(--ink-3);
		letter-spacing: 0.06em;
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
	}
	.comp-name {
		margin: 18px 0 8px;
		font-size: 22px;
		font-weight: 500;
		letter-spacing: -0.01em;
		transition: color 0.18s;
	}
	.comp-desc {
		margin: 0;
		font-size: 13px;
		line-height: 1.6;
		color: var(--ink-2);
		flex: 1;
	}
	.comp-foot {
		margin-top: 16px;
		padding-top: 10px;
		border-top: 1px dotted var(--line);
		display: flex;
		justify-content: space-between;
		align-items: center;
	}
	.go {
		transition: transform 0.18s;
	}
	.comp:hover .go {
		transform: translateX(4px);
	}
	.stamp {
		font-size: 10px;
		letter-spacing: 0.16em;
		color: #ffd29a;
		border: 1.5px solid #ffd29a;
		padding: 2px 6px;
		transform: rotate(-2deg);
		display: inline-block;
	}
	.comp-callout {
		display: none;
	}
	.legend-li {
		display: none;
	}
	.legend {
		position: relative;
		height: 100%;
		min-height: 180px;
		display: flex;
		flex-direction: column;
		justify-content: flex-end;
		gap: 10px;
		padding: 16px 18px;
		border: 1px dotted var(--line);
		font-size: 12px;
		color: var(--ink-2);
	}
	.lg-row {
		display: flex;
		align-items: center;
		gap: 10px;
	}
	.lg-sw {
		width: 28px;
		height: 14px;
		border: 1px solid var(--ink);
		flex: none;
	}
	.lg-sw.dashed {
		border-style: dashed;
		border-color: var(--line);
	}
	.lg-sw.hot {
		border-color: var(--hot);
		box-shadow: 0 0 8px rgba(127, 244, 255, 0.4);
	}
	.legend-ch {
		position: absolute;
		top: 16px;
		right: 18px;
		width: 44px;
		height: 44px;
		opacity: 0.6;
	}

	/* BOM */
	.bom-wrap {
		overflow-x: auto;
		background: rgba(8, 36, 84, 0.45);
	}
	.bom {
		width: 100%;
		border-collapse: collapse;
		font-size: 13px;
	}
	.bom th {
		text-align: left;
		font-size: 10px;
		font-weight: 500;
		letter-spacing: 0.16em;
		color: var(--ink-3);
		padding: 10px 12px;
		border-bottom: 2px solid var(--ink);
		white-space: nowrap;
	}
	.bom td {
		padding: 12px;
		border-bottom: 1px solid var(--line);
		vertical-align: top;
	}
	.bom tbody tr:last-child td {
		border-bottom: 0;
	}
	.bom td + td,
	.bom th + th {
		border-left: 1px solid var(--line);
	}
	.bom tbody tr {
		transition: background-color 0.15s;
	}
	.bom tbody tr:hover {
		background: rgba(40, 150, 220, 0.16);
	}
	.bom tbody tr:hover .bom-link {
		color: var(--hot);
	}
	.c-item,
	.c-qty {
		width: 1%;
		text-align: center !important;
		color: var(--ink-2);
	}
	.c-part {
		white-space: nowrap;
		color: var(--ink-2);
		display: none;
	}
	.c-desc {
		display: none;
		color: var(--ink-2);
		line-height: 1.55;
	}
	.bom-link {
		font-weight: 500;
		border-bottom: 1px solid var(--line);
		word-break: break-word;
	}
	.bom-desc-sm {
		display: block;
		margin-top: 6px;
		font-size: 12px;
		line-height: 1.5;
		color: var(--ink-2);
	}

	/* bottom */
	.bottom {
		display: grid;
		grid-template-columns: 1fr;
		gap: 36px;
		margin-top: 0;
	}
	.refs {
		list-style: none;
		margin: 0;
		padding: 0;
	}
	.refs li {
		display: flex;
		flex-direction: column;
		gap: 4px;
		padding: 12px 0;
		border-bottom: 1px dotted var(--line);
	}
	.ref-id {
		font-size: 10px;
		letter-spacing: 0.16em;
		color: var(--ink-3);
	}
	.ref-title {
		font-size: 16px;
		line-height: 1.4;
	}
	.ref-meta {
		font-size: 12px;
		color: var(--ink-2);
	}
	.notes {
		margin: 18px 0 0;
		font-size: 15px;
		line-height: 1.6;
	}

	.tblock {
		display: grid;
		grid-template-columns: repeat(2, 1fr);
		border: 2px solid var(--ink);
		align-self: end;
		background: rgba(8, 36, 84, 0.6);
	}
	.tb-cell {
		display: flex;
		flex-direction: column;
		gap: 4px;
		padding: 8px 10px;
		border-right: 1px solid var(--line);
		border-bottom: 1px solid var(--line);
	}
	.tb-cell:nth-child(2n + 1):not(.tb-title):not(.tb-wide) {
		border-right: 1px solid var(--line);
	}
	.tb-title,
	.tb-wide,
	.tb-appr {
		grid-column: 1 / -1;
	}
	.tb-title {
		border-bottom: 2px solid var(--ink);
	}
	.tb-big {
		font-size: 22px;
		font-weight: 600;
		letter-spacing: 0.24em;
	}
	.tb-val {
		font-size: 13px;
		font-weight: 500;
		letter-spacing: 0.06em;
	}
	.tb-sig {
		font-size: 20px;
		color: var(--ink);
		line-height: 1.1;
	}
	.tblock > :last-child {
		border-bottom: 0;
	}

	/* ---------- ≥ 720px ---------- */
	@media (min-width: 720px) {
		.bp {
			padding: 40px 32px;
		}
		.inner {
			padding: 48px 48px 40px;
		}
		.sec {
			margin-top: 72px;
		}
		.sec-note {
			display: inline;
		}
		.role {
			grid-template-columns: 2fr 1.4fr 1fr;
		}
		.role-cell + .role-cell {
			border-top: 0;
			border-left: 1px solid var(--line);
		}
		.comp-grid {
			grid-template-columns: repeat(2, 1fr);
			gap: 36px 32px;
		}
		.c-part,
		.c-desc {
			display: table-cell;
		}
		.bom-desc-sm {
			display: none;
		}
		.bom td,
		.bom th {
			padding-left: 16px;
			padding-right: 16px;
		}
		.tblock {
			grid-template-columns: repeat(4, 1fr);
		}
		.tb-wide,
		.tb-dept {
			grid-column: span 2;
		}
		.tb-appr {
			grid-column: auto;
		}

		/* zone markers */
		.zones-x,
		.zones-y {
			display: flex;
			position: absolute;
			font-size: 10px;
			color: var(--ink-3);
			pointer-events: none;
		}
		.zones-x {
			top: 0;
			left: 0;
			right: 0;
			height: 18px;
			border-bottom: 1px solid var(--line);
		}
		.zones-x span {
			flex: 1;
			text-align: center;
			line-height: 18px;
		}
		.zones-x span + span {
			border-left: 1px solid var(--line);
		}
		.zones-y {
			flex-direction: column;
			top: 18px;
			bottom: 0;
			left: 0;
			width: 18px;
			border-right: 1px solid var(--line);
		}
		.zones-y span {
			flex: 1;
			display: grid;
			place-items: center;
		}
		.zones-y span + span {
			border-top: 1px solid var(--line);
		}
		.inner {
			margin-left: 18px;
			padding-top: 60px;
		}
	}

	/* ---------- ≥ 1024px ---------- */
	@media (min-width: 1024px) {
		.comp-grid {
			grid-template-columns: repeat(3, 1fr);
		}
		.legend-li {
			display: block;
		}
		.comp-grid {
			row-gap: 44px;
		}
		.sec-head {
			margin-bottom: 36px;
		}
		.comp-callout {
			display: inline-flex;
			align-items: flex-end;
			gap: 4px;
			position: absolute;
			top: -24px;
			right: 4px;
			font-size: 14px;
			pointer-events: none;
		}
		.comp-callout svg {
			width: 30px;
			height: 16px;
			fill: none;
			stroke: var(--ink-2);
			stroke-width: 1;
			order: 2;
		}
		.comp-callout svg circle {
			fill: var(--ink-2);
		}
		.bottom {
			grid-template-columns: 1fr 480px;
			gap: 56px;
			align-items: end;
		}
		.tblock {
			margin-right: -48px;
			margin-bottom: -40px;
			border-right: 0;
			border-bottom: 0;
		}
	}

	@media (max-width: 480px) {
		.sec-title {
			letter-spacing: 0.12em;
		}
		.hide-sm {
			display: none;
		}
		.dim-name .dim-label {
			font-size: 9px;
			padding: 0 6px;
		}
	}

	@media (prefers-reduced-motion: reduce) {
		.bp *,
		.bp *::before,
		.bp *::after {
			transition: none !important;
			animation: none !important;
		}
	}
</style>
