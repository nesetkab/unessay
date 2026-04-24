<script lang="ts">
	import { onMount } from 'svelte';

	let mounted = $state(false);
	let scrollY = $state(0);
	let visibleArtifacts = $state<Set<string>>(new Set());
	let activeArtifact = $state('001');
	let chartAnimated = $state(false);
	let navOpen = $state(false);

	const botData = [
		{ year: '2020', pct: 41.2, width: 41.2 },
		{ year: '2021', pct: 44.8, width: 44.8 },
		{ year: '2022', pct: 47.4, width: 47.4 },
		{ year: '2023', pct: 57.3, width: 57.3 },
		{ year: '2024', pct: 64.1, width: 64.1 },
		{ year: '2025', pct: 71.8, width: 71.8 },
	];

	onMount(() => {
		mounted = true;

		const observer = new IntersectionObserver(
			(entries) => {
				for (const entry of entries) {
					if (entry.isIntersecting) {
						visibleArtifacts = new Set([...visibleArtifacts, entry.target.id]);
						observer.unobserve(entry.target);
					}
				}
			},
			{ threshold: 0.08, rootMargin: '0px 0px -40px 0px' }
		);

		const activeObserver = new IntersectionObserver(
			(entries) => {
				for (const entry of entries) {
					if (entry.isIntersecting) {
						const id = entry.target.id.replace('artifact-', '');
						activeArtifact = id;
					}
				}
			},
			{ threshold: 0, rootMargin: '-20% 0px -60% 0px' }
		);

		document.querySelectorAll('.artifact').forEach((el) => {
			observer.observe(el);
			activeObserver.observe(el);
		});

		// chart animation observer
		const chartEl = document.getElementById('artifact-003');
		if (chartEl) {
			const chartObs = new IntersectionObserver(
				(entries) => {
					if (entries[0].isIntersecting) {
						chartAnimated = true;
						chartObs.disconnect();
					}
				},
				{ threshold: 0.3 }
			);
			chartObs.observe(chartEl);
		}

		return () => {
			observer.disconnect();
			activeObserver.disconnect();
		};
	});

	function handleNavClick() {
		navOpen = false;
	}

	const artifacts = [
		{
			id: '001',
			title: 'The CAPTCHA Graveyard',
			date: 'c. 2021–2024',
			medium: 'Distorted text, image grids, behavioral heuristics',
			dimensions: '300×80px (standard), variable (reCAPTCHA v3)',
			provenance: 'Google LLC → Cloudflare → deprecated',
			condition: 'Severely degraded. No longer functional against intended targets.',
			description: `For roughly two decades, CAPTCHAs served as the primary border checkpoint between human and machine traffic. Users were asked to identify fire hydrants, crosswalks, and storefronts — a kind of involuntary labor that simultaneously trained the computer vision systems that would eventually render the test obsolete.`,
			note: `By 2024, commercial CAPTCHA-solving services advertised 99.1% accuracy at $2.99 per 1,000 solves. The median human accuracy had dropped to 85%. The test could no longer tell who it was testing.`,
			evidence: [
				'SELECT ALL SQUARES WITH ████████ ',
				'                                 ',
				'  ┌────┬────┬────┐               ',
				'  │    │ ░░ │    │               ',
				'  ├────┼────┼────┤               ',
				'  │ ░░ │    │ ░░ │               ',
				'  ├────┼────┼────┤               ',
				'  │    │ ░░ │    │               ',
				'  └────┴────┴────┘               ',
				'                                 ',
				'  [SKIP]          [VERIFY]       ',
				'                                 ',
				'  ── solving time: 0.003s ──     ',
				'  ── confidence: 99.7% ──        ',
				'  ── solver: GPT-4V proxy ──     ',
			]
		},
		{
			id: '002',
			title: 'Content Farm Specimen (Preserved)',
			date: '2023',
			medium: 'AI-generated text, stock photography, programmatic ad placement',
			dimensions: '~1,200 words, 14 ad slots, 0 original thoughts',
			provenance: 'Unknown LLC, registered in Delaware. Domain purchased 2022, abandoned 2024.',
			condition: 'Intact. Typical of approximately 4.3 million similar pages indexed that year.',
			description: `This specimen represents the dominant literary form of the early 2020s internet: the SEO-optimized AI article. Written by no one, for no one, about nothing — but structured precisely to appear in search results for queries like "best quiet blenders 2023" or "how to tell if your dog is happy."`,
			note: `The article contains 47 uses of the phrase "in this article, we will explore." It references studies that do not exist. The author bio describes "Sarah Mitchell, a passionate lifestyle writer with over 10 years of experience" — a person who has never existed in any public record.`,
			evidence: [
				'─────────────────────────────────────',
				'bestquietblenders2023.com/top-picks  ',
				'─────────────────────────────────────',
				'                                     ',
				'  10 BEST QUIET BLENDERS FOR YOUR    ',
				'  KITCHEN (2023 BUYER\'S GUIDE)       ',
				'                                     ',
				'  By Sarah Mitchell | Oct 14, 2023   ',
				'  ⏱ 8 min read                      ',
				'                                     ',
				'  Are you looking for the best quiet ',
				'  blender? In this article, we will  ',
				'  explore the top quiet blenders     ',
				'  available on the market today.     ',
				'                                     ',
				'  ▓▓▓▓▓▓▓▓▓▓▓ AD UNIT ▓▓▓▓▓▓▓▓▓▓▓ ',
				'  ▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓ ',
				'                                     ',
				'  What Is a Quiet Blender?           ',
				'  A quiet blender is a blender that  ',
				'  is quiet. Many people prefer quiet ',
				'  blenders because they are not loud.',
				'                                     ',
				'  [... 1,147 more words ...]         ',
			]
		},
		{
			id: '003',
			title: 'Dead Internet Index, 2020–2025',
			date: '2020–2025',
			medium: 'Aggregated traffic analytics, bot detection logs',
			dimensions: 'Global web traffic dataset, ~2.1PB compressed',
			provenance: 'Compiled from Cloudflare, Akamai, and Imperva annual reports.',
			condition: 'Data intact. Conclusions disputed by platform operators.',
			description: `The "Dead Internet Theory" — once a fringe conspiracy — proposed that the majority of online activity was generated by bots, not humans. By 2024, it had become less a theory and more a measurement problem. These figures, drawn from infrastructure companies that route the actual traffic, tell a story that platform companies preferred not to tell themselves.`,
			note: `Note the inflection point in 2023, which coincides with the widespread availability of large language models capable of generating human-passing text at scale. After this point, the distinction between "bot traffic" and "human traffic" becomes increasingly semantic.`,
			evidence: null
		},
		{
			id: '004',
			title: 'Synthetic Conversation (Reddit, r/AskReddit)',
			date: 'November 2024',
			medium: 'Forum post, auto-generated replies, engagement farming',
			dimensions: '1 thread, 847 replies, 12.4k upvotes',
			provenance: 'Archived via Pushshift before API restrictions.',
			condition: 'Thread still accessible. 94% of participating accounts suspended within 60 days.',
			description: `This thread represents a common pattern identified by researchers at Stanford and Indiana University: a question posted by a bot account, answered by other bot accounts, upvoted by bot networks, and surfaced to human users via algorithmic recommendation. The humans who eventually participated did not know they were joining a synthetic conversation.`,
			note: `The original poster's account was 3 days old at time of posting. It would go on to post 2,100 threads in 14 days before suspension. The top-voted reply — "I'm not crying, you're crying 😭" — appeared verbatim in 340 other threads that month.`,
			evidence: [
				'  r/AskReddit                         ',
				'  u/genuine_life_2847 · 3d             ',
				'                                       ',
				'  What\'s something that seems normal   ',
				'  now but will seem insane in 50        ',
				'  years?                               ',
				'                                       ',
				'  ↑ 12.4k  💬 847  ↗ Share             ',
				'  ──────────────────────────────────── ',
				'  u/happy_thoughts_99 · 2d             ',
				'  Having to prove you\'re not a robot   ',
				'  every time you want to read an       ',
				'  article lol                          ',
				'    ↑ 4.2k                             ',
				'                                       ',
				'  u/realhumanperson_x · 2d             ',
				'  I\'m not crying, you\'re crying 😭    ',
				'    ↑ 2.1k                             ',
				'                                       ',
				'  [94% of accounts: suspended]         ',
			]
		},
		{
			id: '005',
			title: 'The Obituary of the Hyperlink',
			date: 'c. 2022–2025',
			medium: 'Browser telemetry, search engine behavior logs',
			dimensions: 'N/A (behavioral pattern)',
			provenance: 'Compiled from Google Search Console data and Similarweb analytics.',
			condition: 'Ongoing. Terminal.',
			description: `The hyperlink — the foundational technology of the World Wide Web — died slowly between 2022 and 2025. Not from disuse, but from irrelevance. As AI-generated summaries replaced the need to visit source pages, click-through rates from search results collapsed. Websites that had spent decades building audiences found their traffic cut by 40–70% as search engines began answering queries directly, often by summarizing those very websites' content without attribution.`,
			note: `The term "zero-click search" was coined by SparkToro's Rand Fishkin in 2019, when 50% of Google searches ended without a click to any website. By 2025, that number reached 74%. The web still existed. People just stopped going to it.`,
			evidence: [
				'  GOOGLE SEARCH: "what is serotonin"   ',
				'  ────────────────────────────────────  ',
				'                                        ',
				'  ┌─ AI Overview ───────────────────┐   ',
				'  │                                 │   ',
				'  │ Serotonin is a neurotransmitter  │   ',
				'  │ that plays a key role in mood,   │   ',
				'  │ sleep, digestion, and more.      │   ',
				'  │ It is sometimes called the       │   ',
				'  │ "happy chemical" because it      │   ',
				'  │ contributes to feelings of       │   ',
				'  │ well-being and happiness.        │   ',
				'  │                                 │   ',
				'  │ Sources: WebMD, Mayo Clinic,     │   ',
				'  │ Cleveland Clinic                 │   ',
				'  └─────────────────────────────────┘   ',
				'                                        ',
				'  ░░ 10 blue links (below the fold) ░░  ',
				'  ░░ avg CTR: 8.7% (down from 31%) ░░  ',
			]
		},
		{
			id: '006',
			title: 'Amazon Review Stratification Core Sample',
			date: '2023–2024',
			medium: 'Product reviews, verified and unverified purchase labels',
			dimensions: '1 product listing, 2,847 reviews analyzed',
			provenance: 'Amazon.com marketplace, US region.',
			condition: 'Representative. Pattern observed across 67% of electronics category listings.',
			description: `By 2024, the Amazon review ecosystem had stratified into distinct geological layers, each representing a different era of synthetic content. Early reviews (2019–2021) show telltale signs of review farms: broken English, excessive exclamation marks, five stars with no detail. Later layers (2023–2024) are far more sophisticated — grammatically perfect, emotionally calibrated, indistinguishable from genuine human opinion at the individual level. Only statistical analysis reveals the pattern: identical sentiment curves, suspiciously uniform posting times, and the same 12 adjectives rotating across thousands of "unique" reviews.`,
			note: `The product in question — a $34.99 Bluetooth speaker — had more written words of review (418,000) than Moby Dick (209,000). The speaker itself was adequate.`,
			evidence: [
				'  ★★★★★  REVIEW STRATIFICATION       ',
				'  ──────────────────────────────────  ',
				'  LAYER 1 (2019–2021) — FARM ERA      ',
				'  "Very good product!!! Best buy ever ',
				'   I am very happy recommend to all   ',
				'   my friend!!!" — verified purchase  ',
				'                                      ',
				'  LAYER 2 (2022–2023) — TRANSITION    ',
				'  "Decent speaker for the price. Bass ',
				'   is a bit tinny but works fine for  ',
				'   my kitchen." — unverified          ',
				'                                      ',
				'  LAYER 3 (2023–2024) — LLM ERA       ',
				'  "I\'ve been using this speaker for   ',
				'   about three weeks now and honestly ',
				'   it\'s exceeded my expectations.     ',
				'   The sound is warm and balanced,    ',
				'   and..." — verified purchase        ',
				'                                      ',
				'  TOTAL WORD COUNT: 418,000           ',
				'  MOBY DICK WORD COUNT: 209,000       ',
			]
		},
		{
			id: '007',
			title: 'LinkedIn Thought Leadership Generator Output',
			date: '2024',
			medium: 'Social media post, engagement bait, algorithmic amplification',
			dimensions: '1 post, 47,000 impressions, 312 "Insightful" reactions',
			provenance: 'LinkedIn.com, professional networking platform.',
			condition: 'Ubiquitous. This post is indistinguishable from 40,000 others posted that week.',
			description: `LinkedIn became, by 2024, the largest open-air demonstration of AI-generated content operating in plain sight. The platform's algorithm rewarded a specific format — short paragraphs, line breaks after every sentence, a "vulnerable" opening hook, and a triumphant conclusion — that was trivially easy for language models to replicate. The result was a feed of interchangeable motivational content, each post performing authenticity while being authored by no one in particular.`,
			note: `The engagement on this post was genuine. Real humans read it, related to it, and pressed "Insightful." The fact that it was generated by a $20/month AI writing tool did not diminish its emotional impact. This is perhaps the most unsettling artifact in the collection.`,
			evidence: [
				'  ╔═══════════════════════════════╗  ',
				'  ║  🔵 Tyler Bransen             ║  ',
				'  ║  Growth Mindset Evangelist |   ║  ',
				'  ║  Ex-Google | TEDx Speaker      ║  ',
				'  ╚═══════════════════════════════╝  ',
				'                                     ',
				'  I got fired last Tuesday.           ',
				'                                     ',
				'  And it was the best thing that      ',
				'  ever happened to me.                ',
				'                                     ',
				'  Here\'s what nobody tells you about ',
				'  losing your job:                    ',
				'                                     ',
				'  1. You find out who your real       ',
				'     friends are                      ',
				'  2. You discover what you actually   ',
				'     care about                       ',
				'  3. You stop performing and start    ',
				'     living                           ',
				'                                     ',
				'  Agree? ♻ Repost if this resonated. ',
				'                                     ',
				'  💬 312  🔄 89  👍 2.1K             ',
			]
		},
		{
			id: '008',
			title: 'The Last Human Comment (Attributed)',
			date: 'Unconfirmed, estimated late 2025',
			medium: 'Blog comment, plaintext, no engagement metrics',
			dimensions: '43 words',
			provenance: 'Personal blog, hosted on shared cPanel server. Domain expired 2026.',
			condition: 'Fragment. Server offline. Preserved via Wayback Machine snapshot.',
			description: `This artifact is, admittedly, apocryphal. We cannot confirm it is the "last" human comment posted to the open internet — that distinction is both impossible to verify and somewhat beside the point. What we can say is that it represents a class of online expression that became vanishingly rare by late 2025: an unsolicited, non-monetized, non-algorithmic human thought, posted to a place where no one was likely to read it.`,
			note: `The blog had 11 posts, all about amateur radio. The last post received this single comment, eight days after publication. The commenter's handle, "wx4sky," corresponds to a real amateur radio callsign registered in North Carolina. We have chosen not to contact them.`,
			evidence: [
				'                                       ',
				'  ┌───────────────────────────────────┐ ',
				'  │  wx4sky                           │ ',
				'  │  December 3, 2025 at 4:17 PM      │ ',
				'  │                                   │ ',
				'  │  Hey, great writeup on the 40m    │ ',
				'  │  antenna mod. I tried something   │ ',
				'  │  similar last winter and got       │ ',
				'  │  about 2db gain on the east        │ ',
				'  │  coast path. Let me know if you    │ ',
				'  │  ever get the 20m version          │ ',
				'  │  working.                          │ ',
				'  │                                   │ ',
				'  │  73,                               │ ',
				'  │  wx4sky                            │ ',
				'  └───────────────────────────────────┘ ',
				'                                       ',
				'  ── 0 likes · 0 replies ──            ',
			]
		}
	];
</script>

<svelte:window bind:scrollY={scrollY} />

<!-- SIDEBAR NAV (desktop) -->
<nav class="sidebar" class:mounted aria-label="Artifact navigation">
	<div class="sidebar-header">
		<span class="sidebar-label">CATALOG INDEX</span>
	</div>
	<ol class="sidebar-list">
		{#each artifacts as artifact}
			<li>
				<a
					href="#artifact-{artifact.id}"
					class="sidebar-link"
					class:active={activeArtifact === artifact.id}
				>
					<span class="sidebar-num">{artifact.id}</span>
					<span class="sidebar-title">{artifact.title}</span>
				</a>
			</li>
		{/each}
	</ol>
	<div class="sidebar-footer">
		<span>WING B · GALLERY 3</span>
	</div>
</nav>

<!-- MOBILE NAV -->
<button class="mobile-nav-toggle" class:open={navOpen} onclick={() => navOpen = !navOpen} aria-label="Toggle navigation">
	<span class="toggle-label">INDEX</span>
	<span class="toggle-indicator">{navOpen ? '−' : '+'}</span>
</button>

{#if navOpen}
	<div class="mobile-nav" role="navigation">
		<ol>
			{#each artifacts as artifact}
				<li>
					<a
						href="#artifact-{artifact.id}"
						class:active={activeArtifact === artifact.id}
						onclick={handleNavClick}
					>
						<span class="mn-num">{artifact.id}</span>
						{artifact.title}
					</a>
				</li>
			{/each}
		</ol>
	</div>
{/if}

<!-- MAIN CONTENT -->
<div class="catalog" class:mounted>

	<header class="catalog-header">
		<div class="institution">
			<span class="inst-label">THE INTERNET ARCHIVE</span>
			<span class="inst-sub">DEPARTMENT OF DIGITAL ARCHAEOLOGY</span>
		</div>
		<div class="catalog-meta">
			<span>EXHIBIT CATALOG No. 7</span>
			<span>WING B, GALLERY 3</span>
			<span>PERMANENT COLLECTION</span>
		</div>
	</header>

	<div class="title-block">
		<div class="title-rule"></div>
		<h1>Digital Ruins</h1>
		<p class="subtitle">Internet Artifacts, 2020–2025</p>
		<div class="title-rule"></div>
		<p class="curator-note">
			Curated by the Department of Digital Archaeology<br />
			Catalog published on the occasion of the exhibit's opening, Autumn 2037
		</p>
	</div>

	<div class="foreword">
		<h2>Foreword</h2>
		<p>
			The internet of the early 2020s was not what it appeared to be. Beneath the surface of
			what most users experienced as a network of human communication, a vast and mostly invisible
			infrastructure of automated systems was generating, amplifying, and engaging with content at
			a scale that dwarfed human participation.
		</p>
		<p>
			This exhibit presents eight artifacts from that period — each one a piece of evidence
			in what we now understand as the great transition: the years in which the internet stopped
			being a place humans wrote to each other, and became a place machines wrote to humans,
			pretending to be other humans.
		</p>
		<p>
			The objects in this collection are not rare. That is precisely the point. They are
			representative of systems that operated at planetary scale, affecting billions of
			interactions per day. We preserve them here not because they are exceptional, but
			because they were, at the time, so ordinary that almost no one thought to question them.
		</p>
		<p class="foreword-sig">
			— Dr. R. Vasquez-Okafor<br />
			<span class="fg-dim">Director, Dept. of Digital Archaeology</span><br />
			<span class="fg-dim">September 2037</span>
		</p>
	</div>

	<div class="toc">
		<h2>Contents</h2>
		<ol>
			{#each artifacts as artifact}
				<li>
					<a href="#artifact-{artifact.id}">
						<span class="toc-num">{artifact.id}</span>
						<span class="toc-title">{artifact.title}</span>
						<span class="toc-dots"></span>
						<span class="toc-date">{artifact.date}</span>
					</a>
				</li>
			{/each}
		</ol>
	</div>

	<main class="artifacts">
		{#each artifacts as artifact, i}
			<article
				class="artifact"
				class:artifact-visible={visibleArtifacts.has(`artifact-${artifact.id}`)}
				id="artifact-{artifact.id}"
			>
				<div class="artifact-header">
					<span class="artifact-num">ARTIFACT {artifact.id}</span>
					<div class="artifact-title-block">
						<h2>{artifact.title}</h2>
					</div>
				</div>

				<div class="artifact-meta">
					<div class="meta-row">
						<span class="meta-label">Date</span>
						<span class="meta-value">{artifact.date}</span>
					</div>
					<div class="meta-row">
						<span class="meta-label">Medium</span>
						<span class="meta-value">{artifact.medium}</span>
					</div>
					<div class="meta-row">
						<span class="meta-label">Dimensions</span>
						<span class="meta-value">{artifact.dimensions}</span>
					</div>
					<div class="meta-row">
						<span class="meta-label">Provenance</span>
						<span class="meta-value">{artifact.provenance}</span>
					</div>
					<div class="meta-row meta-condition">
						<span class="meta-label">Condition</span>
						<span class="meta-value condition-value">{artifact.condition}</span>
					</div>
				</div>

				<div class="artifact-body">
					<p>{artifact.description}</p>
				</div>

				<!-- SPECIAL: Animated chart for artifact 003 -->
				{#if artifact.id === '003'}
					<div class="artifact-evidence display-case">
						<div class="evidence-label">EXHIBIT 003-A — PRIMARY EVIDENCE</div>
						<div class="chart-display">
							<div class="chart-header">ESTIMATED BOT TRAFFIC AS % OF TOTAL</div>
							<div class="chart-separator">───────────────────────────────────</div>
							{#each botData as row, j}
								<div class="chart-row">
									<span class="chart-year">{row.year}</span>
									<span class="chart-pipe">│</span>
									<span class="chart-bar-track">
										<span
											class="chart-bar-fill"
											class:inflection={j >= 3}
											style="width: {chartAnimated ? row.width : 0}%; transition-delay: {chartAnimated ? j * 0.18 : 0}s;"
										></span>
									</span>
									<span class="chart-pct" class:chart-pct-visible={chartAnimated} style="transition-delay: {j * 0.18 + 0.4}s;">
										{row.pct}%
									</span>
								</div>
							{/each}
							<div class="chart-separator">───────────────────────────────────</div>
							<div class="chart-annotation" class:chart-annotation-visible={chartAnimated}>
								<span class="chart-arrow">▲</span>
								<span>LLM inflection point</span>
							</div>
							<div class="chart-source">
								source: Imperva Bad Bot Report,<br />
								Cloudflare Radar, Akamai SOTI
							</div>
						</div>
					</div>
				{:else if artifact.evidence}
					<div class="artifact-evidence display-case">
						<div class="evidence-label">EXHIBIT {artifact.id}-A — PRIMARY EVIDENCE</div>
						<pre class="evidence-block">{artifact.evidence.join('\n')}</pre>
					</div>
				{/if}

				{#if artifact.note}
					<aside class="artifact-note">
						<span class="note-marker">✱</span>
						<div class="note-content">
							<span class="note-label">Curator's Note</span>
							<p>{artifact.note}</p>
						</div>
					</aside>
				{/if}

				{#if i < artifacts.length - 1}
					<div class="artifact-separator">
						<span>· · ·</span>
					</div>
				{/if}
			</article>
		{/each}
	</main>

	<footer class="catalog-footer">
		<div class="footer-rule"></div>
		<div class="colophon">
			<p>
				DIGITAL RUINS: INTERNET ARTIFACTS 2020–2025<br />
				<span class="fg-dim">Exhibit Catalog No. 7 · Wing B, Gallery 3</span>
			</p>
			<p class="fg-dim">
				The Internet Archive · Department of Digital Archaeology<br />
				Published Autumn 2037 · First Edition
			</p>
		</div>

		<div class="thesis">
			<p>
				All artifacts in this collection are preserved under the Digital Heritage Act of 2031.
				Reproduction permitted for educational and research purposes.
			</p>
			<p class="thesis-line">
				The curators wish to note that no artificial intelligence was consulted
				in the writing of this catalog, though we acknowledge the irony of that
				claim may itself be the exhibit's final artifact.
			</p>
		</div>

		<div class="footer-rule"></div>
		<p class="fin">FIN</p>
	</footer>
</div>

<style>
	/* ── LAYOUT ── */
	.catalog {
		max-width: 720px;
		margin: 0 auto;
		padding: 4rem 2rem 6rem;
		opacity: 0;
		transition: opacity 0.8s ease;
	}
	.catalog.mounted {
		opacity: 1;
	}

	/* ── SIDEBAR (desktop) ── */
	.sidebar {
		position: fixed;
		top: 1.75rem;
		left: 0;
		width: 220px;
		height: calc(100vh - 1.75rem);
		padding: 2rem 1.25rem;
		border-right: 1px solid var(--border);
		display: flex;
		flex-direction: column;
		z-index: 100;
		opacity: 0;
		transition: opacity 0.8s ease 0.3s;
		overflow-y: auto;
	}
	.sidebar.mounted {
		opacity: 1;
	}
	.sidebar-header {
		margin-bottom: 1.5rem;
		padding-bottom: 1rem;
		border-bottom: 1px solid var(--border);
	}
	.sidebar-label {
		font-size: 0.5625rem;
		letter-spacing: 0.25em;
		color: var(--fg-faint);
	}
	.sidebar-list {
		list-style: none;
		flex: 1;
	}
	.sidebar-link {
		display: flex;
		align-items: baseline;
		gap: 0.5rem;
		padding: 0.4rem 0;
		border-bottom: none;
		color: var(--fg-faint);
		font-size: 0.6875rem;
		line-height: 1.4;
		transition: color 0.25s;
	}
	.sidebar-link:hover {
		color: var(--fg-dim);
	}
	.sidebar-link.active {
		color: var(--fg);
	}
	.sidebar-link.active .sidebar-num {
		color: var(--accent);
	}
	.sidebar-num {
		font-size: 0.5625rem;
		min-width: 1.5rem;
		color: var(--fg-faint);
		transition: color 0.25s;
	}
	.sidebar-title {
		flex: 1;
		overflow: hidden;
		text-overflow: ellipsis;
		white-space: nowrap;
	}
	.sidebar-footer {
		padding-top: 1rem;
		border-top: 1px solid var(--border);
		font-size: 0.5rem;
		letter-spacing: 0.2em;
		color: var(--fg-faint);
		margin-top: auto;
	}

	/* ── MOBILE NAV ── */
	.mobile-nav-toggle {
		display: none;
		position: fixed;
		top: 1.75rem;
		left: 0;
		right: 0;
		z-index: 200;
		background: var(--bg);
		border: none;
		border-bottom: 1px solid var(--border);
		color: var(--fg-dim);
		font-family: var(--mono);
		font-size: 0.625rem;
		letter-spacing: 0.2em;
		padding: 0.75rem 1.25rem;
		cursor: pointer;
		justify-content: space-between;
		align-items: center;
	}
	.toggle-indicator {
		font-size: 0.875rem;
		color: var(--fg-faint);
	}
	.mobile-nav {
		display: none;
		position: fixed;
		top: 4.25rem;
		left: 0;
		right: 0;
		z-index: 199;
		background: var(--bg);
		border-bottom: 1px solid var(--border);
		padding: 0.5rem 1.25rem 1rem;
		max-height: 60vh;
		overflow-y: auto;
	}
	.mobile-nav ol {
		list-style: none;
	}
	.mobile-nav a {
		display: block;
		padding: 0.5rem 0;
		border-bottom: none;
		color: var(--fg-dim);
		font-size: 0.6875rem;
	}
	.mobile-nav a.active {
		color: var(--accent);
	}
	.mn-num {
		color: var(--fg-faint);
		margin-right: 0.5rem;
		font-size: 0.5625rem;
	}

	/* ── HEADER ── */
	.catalog-header {
		display: flex;
		justify-content: space-between;
		align-items: flex-start;
		margin-bottom: 4rem;
		padding-bottom: 1.5rem;
		border-bottom: 1px solid var(--border);
	}
	.institution {
		display: flex;
		flex-direction: column;
		gap: 0.25rem;
	}
	.inst-label {
		font-size: 0.6875rem;
		letter-spacing: 0.2em;
		color: var(--fg-dim);
		font-weight: 500;
	}
	.inst-sub {
		font-size: 0.5625rem;
		letter-spacing: 0.15em;
		color: var(--fg-faint);
	}
	.catalog-meta {
		display: flex;
		flex-direction: column;
		align-items: flex-end;
		gap: 0.15rem;
		font-size: 0.5625rem;
		letter-spacing: 0.15em;
		color: var(--fg-faint);
	}

	/* ── TITLE ── */
	.title-block {
		text-align: center;
		margin-bottom: 4rem;
	}
	.title-rule {
		height: 1px;
		background: var(--border-light);
		margin: 1.5rem 4rem;
	}
	h1 {
		font-family: var(--serif);
		font-size: 3rem;
		font-weight: 400;
		color: var(--fg);
		letter-spacing: 0.04em;
		margin: 1rem 0 0.25rem;
	}
	.subtitle {
		font-family: var(--serif);
		font-size: 1.125rem;
		font-weight: 400;
		font-style: italic;
		color: var(--fg-dim);
		margin-bottom: 0.5rem;
	}
	.curator-note {
		font-size: 0.6875rem;
		color: var(--fg-faint);
		margin-top: 1.5rem;
		line-height: 1.8;
	}

	/* ── FOREWORD ── */
	.foreword {
		margin-bottom: 4rem;
		padding: 2.5rem 0;
		border-top: 1px solid var(--border);
		border-bottom: 1px solid var(--border);
	}
	.foreword h2 {
		font-family: var(--serif);
		font-size: 1.25rem;
		font-weight: 400;
		font-style: italic;
		color: var(--fg-dim);
		margin-bottom: 1.5rem;
	}
	.foreword p {
		font-family: var(--serif);
		font-size: 1rem;
		line-height: 1.9;
		color: var(--fg);
		margin-bottom: 1rem;
		text-indent: 1.5em;
	}
	.foreword p:first-of-type {
		text-indent: 0;
	}
	.foreword-sig {
		text-indent: 0 !important;
		margin-top: 2rem !important;
		font-style: italic;
	}

	/* ── TABLE OF CONTENTS ── */
	.toc {
		margin-bottom: 4rem;
	}
	.toc h2 {
		font-family: var(--serif);
		font-size: 1rem;
		font-weight: 400;
		font-style: italic;
		color: var(--fg-dim);
		margin-bottom: 1.5rem;
		letter-spacing: 0.05em;
	}
	.toc ol {
		list-style: none;
		padding: 0;
	}
	.toc li {
		border-bottom: 1px solid var(--border);
	}
	.toc li:first-child {
		border-top: 1px solid var(--border);
	}
	.toc a {
		display: flex;
		align-items: baseline;
		padding: 0.6rem 0;
		border-bottom: none;
		color: var(--fg);
		gap: 0.75rem;
		transition: color 0.2s;
	}
	.toc a:hover {
		color: var(--accent);
	}
	.toc-num {
		font-size: 0.625rem;
		color: var(--fg-faint);
		letter-spacing: 0.1em;
		min-width: 1.75rem;
	}
	.toc-title {
		font-size: 0.8125rem;
		white-space: nowrap;
	}
	.toc-dots {
		flex: 1;
		border-bottom: 1px dotted var(--fg-faint);
		min-width: 2rem;
		margin-bottom: 0.25em;
	}
	.toc-date {
		font-size: 0.6875rem;
		color: var(--fg-dim);
		font-style: italic;
		font-family: var(--serif);
		white-space: nowrap;
	}

	/* ── ARTIFACTS ── */
	.artifacts {
		margin-bottom: 4rem;
	}
	.artifact {
		margin-bottom: 0;
		padding-top: 3rem;
		opacity: 0;
		transform: translateY(16px);
		transition: opacity 0.6s ease, transform 0.6s ease;
	}
	.artifact-visible {
		opacity: 1;
		transform: translateY(0);
	}
	.artifact-header {
		margin-bottom: 2rem;
	}
	.artifact-num {
		font-size: 0.5625rem;
		letter-spacing: 0.25em;
		color: var(--fg-faint);
		display: block;
		margin-bottom: 0.5rem;
	}
	.artifact-title-block h2 {
		font-family: var(--serif);
		font-size: 1.5rem;
		font-weight: 400;
		color: var(--fg);
		line-height: 1.4;
	}

	/* ── META ── */
	.artifact-meta {
		margin-bottom: 2rem;
		padding: 1.25rem 0;
		border-top: 1px solid var(--border);
		border-bottom: 1px solid var(--border);
	}
	.meta-row {
		display: flex;
		gap: 1.5rem;
		padding: 0.2rem 0;
	}
	.meta-label {
		min-width: 90px;
		font-size: 0.625rem;
		letter-spacing: 0.15em;
		color: var(--fg-faint);
		text-transform: uppercase;
		flex-shrink: 0;
	}
	.meta-value {
		font-size: 0.8125rem;
		color: var(--fg-dim);
		font-family: var(--serif);
		font-style: italic;
	}

	/* condition hover */
	.meta-condition {
		position: relative;
	}
	.condition-value {
		transition: color 0.3s ease;
		cursor: default;
	}
	.meta-condition:hover .condition-value {
		color: var(--red-bright);
	}
	.meta-condition::after {
		content: 'DEGRADED';
		position: absolute;
		right: 0;
		top: 50%;
		transform: translateY(-50%);
		font-family: var(--mono);
		font-size: 0.5rem;
		letter-spacing: 0.3em;
		color: var(--red);
		opacity: 0;
		transition: opacity 0.3s ease;
		font-style: normal;
	}
	.meta-condition:hover::after {
		opacity: 1;
	}

	/* ── BODY ── */
	.artifact-body {
		margin-bottom: 2rem;
	}
	.artifact-body p {
		font-family: var(--serif);
		font-size: 1rem;
		line-height: 1.85;
		color: var(--fg);
	}

	/* ── DISPLAY CASE (evidence boxes) ── */
	.display-case {
		margin-bottom: 2rem;
		background: var(--bg-inset);
		border: 1px solid var(--border);
		padding: 1.5rem;
		position: relative;
		overflow: hidden;
		animation: crt-flicker 8s infinite;
		box-shadow:
			inset 0 0 30px rgba(0, 0, 0, 0.4),
			inset 0 0 80px rgba(0, 0, 0, 0.15),
			0 0 2px rgba(200, 195, 184, 0.03);
		border-radius: 1px;
	}
	/* scanlines */
	.display-case::before {
		content: '';
		position: absolute;
		inset: 0;
		background: repeating-linear-gradient(
			0deg,
			transparent,
			transparent 2px,
			rgba(255, 255, 255, 0.012) 2px,
			rgba(255, 255, 255, 0.012) 4px
		);
		pointer-events: none;
		z-index: 1;
	}
	/* sweeping scanline */
	.display-case::after {
		content: '';
		position: absolute;
		left: 0;
		right: 0;
		height: 4px;
		background: linear-gradient(
			to bottom,
			transparent,
			rgba(200, 195, 184, 0.04),
			transparent
		);
		animation: scanline-drift 6s linear infinite;
		pointer-events: none;
		z-index: 2;
	}
	/* top label bar */
	.evidence-label {
		font-size: 0.5625rem;
		letter-spacing: 0.2em;
		color: var(--fg-faint);
		margin-bottom: 1rem;
		padding-bottom: 0.5rem;
		border-bottom: 1px solid var(--border);
		position: relative;
		z-index: 3;
	}
	.evidence-block {
		font-family: var(--mono);
		font-size: 0.75rem;
		line-height: 1.5;
		color: var(--fg-dim);
		overflow-x: auto;
		white-space: pre;
		position: relative;
		z-index: 3;
		-webkit-overflow-scrolling: touch;
	}

	/* ── ANIMATED CHART (003) ── */
	.chart-display {
		font-family: var(--mono);
		font-size: 0.75rem;
		line-height: 1.5;
		color: var(--fg-dim);
		position: relative;
		z-index: 3;
	}
	.chart-header {
		font-size: 0.6875rem;
		letter-spacing: 0.05em;
		margin-bottom: 0.25rem;
		color: var(--fg);
	}
	.chart-separator {
		color: var(--fg-faint);
		user-select: none;
	}
	.chart-row {
		display: flex;
		align-items: center;
		gap: 0;
		height: 1.5em;
	}
	.chart-year {
		width: 3.5em;
		text-align: right;
		padding-right: 0.5em;
		flex-shrink: 0;
	}
	.chart-pipe {
		flex-shrink: 0;
		margin-right: 0.5em;
		color: var(--fg-faint);
	}
	.chart-bar-track {
		flex: 1;
		height: 0.875em;
		background: rgba(255, 255, 255, 0.02);
		position: relative;
		overflow: hidden;
	}
	.chart-bar-fill {
		position: absolute;
		top: 0;
		left: 0;
		height: 100%;
		width: 0%;
		background: var(--fg-dim);
		transition: width 0.8s cubic-bezier(0.22, 1, 0.36, 1);
	}
	.chart-bar-fill.inflection {
		background: var(--accent);
	}
	.chart-pct {
		width: 4.5em;
		text-align: right;
		flex-shrink: 0;
		opacity: 0;
		transition: opacity 0.4s ease;
	}
	.chart-pct-visible {
		opacity: 1;
	}
	.chart-annotation {
		padding-top: 0.25rem;
		padding-left: 4em;
		color: var(--accent-dim);
		font-size: 0.625rem;
		opacity: 0;
		transition: opacity 0.5s ease 1.5s;
		display: flex;
		gap: 0.75em;
		align-items: center;
	}
	.chart-annotation-visible {
		opacity: 1;
	}
	.chart-arrow {
		color: var(--accent);
	}
	.chart-source {
		margin-top: 0.75rem;
		font-size: 0.625rem;
		color: var(--fg-faint);
	}

	/* ── CURATOR'S NOTE (marginalia) ── */
	.artifact-note {
		margin-bottom: 2rem;
		display: flex;
		gap: 0.75rem;
		align-items: flex-start;
		position: relative;
	}
	.note-marker {
		font-size: 1rem;
		line-height: 1;
		color: var(--accent-dim);
		flex-shrink: 0;
		margin-top: 0.15rem;
		user-select: none;
	}
	.note-content {
		padding: 1rem 1.25rem;
		background: rgba(212, 168, 83, 0.02);
		border-left: 1px solid var(--accent-dim);
		position: relative;
	}
	.note-label {
		font-size: 0.5625rem;
		letter-spacing: 0.2em;
		color: var(--accent-dim);
		text-transform: uppercase;
		display: block;
		margin-bottom: 0.5rem;
	}
	.artifact-note p {
		font-family: var(--serif);
		font-size: 0.9375rem;
		line-height: 1.8;
		color: var(--fg-dim);
		font-style: italic;
	}

	/* ── SEPARATOR ── */
	.artifact-separator {
		text-align: center;
		padding: 3rem 0 0;
		color: var(--fg-faint);
		letter-spacing: 0.5em;
		font-size: 0.75rem;
	}

	/* ── FOOTER ── */
	.catalog-footer {
		padding-top: 3rem;
	}
	.footer-rule {
		height: 1px;
		background: var(--border);
		margin-bottom: 2rem;
	}
	.colophon {
		text-align: center;
		margin-bottom: 3rem;
	}
	.colophon p {
		font-size: 0.6875rem;
		letter-spacing: 0.1em;
		line-height: 1.8;
		margin-bottom: 1rem;
	}

	/* ── THESIS (the closing statement) ── */
	.thesis {
		margin: 3rem -0.5rem 3rem;
		padding: 2.5rem 2rem;
		border-top: 1px solid var(--accent-dim);
		border-bottom: 1px solid var(--accent-dim);
		text-align: center;
		animation: thesis-pulse 6s ease-in-out infinite;
	}
	.thesis p {
		font-size: 0.6875rem;
		letter-spacing: 0.08em;
		line-height: 2;
		color: var(--fg-dim);
		margin-bottom: 1.5rem;
	}
	.thesis-line {
		font-family: var(--serif);
		font-size: 1.0625rem !important;
		font-style: italic;
		color: var(--fg) !important;
		line-height: 1.9 !important;
		letter-spacing: 0.01em !important;
		max-width: 520px;
		margin-left: auto !important;
		margin-right: auto !important;
		margin-bottom: 0 !important;
	}

	.fin {
		text-align: center;
		margin-top: 2rem;
		font-size: 0.625rem;
		letter-spacing: 0.15em;
		color: var(--fg-faint);
	}

	/* ── UTILITIES ── */
	.fg-dim { color: var(--fg-dim); }

	/* ── DESKTOP LAYOUT (sidebar visible) ── */
	@media (min-width: 1080px) {
		.catalog {
			margin-left: 220px;
			margin-right: auto;
			max-width: 720px;
			padding-left: 3rem;
		}
	}

	/* ── HIDE SIDEBAR ON SMALLER SCREENS ── */
	@media (max-width: 1079px) {
		.sidebar {
			display: none;
		}
	}

	/* ── SHOW MOBILE NAV ── */
	@media (max-width: 1079px) {
		.mobile-nav-toggle {
			display: flex;
		}
		.mobile-nav {
			display: block;
		}
		.catalog {
			padding-top: 5rem;
		}
	}

	/* ── MOBILE ── */
	@media (max-width: 600px) {
		.catalog {
			padding: 4.5rem 1.25rem 4rem;
		}
		.catalog-header {
			flex-direction: column;
			gap: 1rem;
		}
		.catalog-meta {
			align-items: flex-start;
		}
		h1 {
			font-size: 2rem;
		}
		.title-rule {
			margin: 1rem 1rem;
		}
		.meta-row {
			flex-direction: column;
			gap: 0.15rem;
		}
		.meta-label {
			min-width: unset;
		}
		/* evidence boxes: scale down and scroll */
		.display-case {
			padding: 1rem 0.75rem;
			margin-left: -0.5rem;
			margin-right: -0.5rem;
		}
		.evidence-block {
			font-size: 0.5625rem;
			line-height: 1.45;
		}
		/* chart responsive */
		.chart-display {
			font-size: 0.625rem;
		}
		.chart-year {
			width: 3em;
		}
		.chart-pct {
			width: 3.5em;
		}
		/* notes stack */
		.artifact-note {
			flex-direction: column;
			gap: 0;
		}
		.note-marker {
			margin-bottom: 0.25rem;
		}
		/* thesis */
		.thesis {
			margin-left: -0.25rem;
			margin-right: -0.25rem;
			padding: 2rem 1rem;
		}
		.thesis-line {
			font-size: 0.9375rem !important;
		}
		.toc-title {
			white-space: normal;
		}
		.toc-dots {
			display: none;
		}
		/* hide condition hover pseudo on touch */
		.meta-condition::after {
			display: none;
		}
	}
</style>
