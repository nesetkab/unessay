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
			description: `For about twenty years, CAPTCHAs were the internet's border checkpoint between humans and machines. You'd get asked to click on fire hydrants, crosswalks, storefronts. Simple enough. But here's the thing - every time you solved one of those little puzzles, you were also training the computer vision systems that would eventually make the test useless. You were teaching the machines how to pass the test designed to keep them out.`,
			note: `By 2024, you could pay a service $2.99 for a thousand CAPTCHA solves, and they'd get 99.1% of them right. The average human was hitting about 85%. So the test meant to tell humans from machines was being failed more often by the humans. Kind of hard to justify keeping it around after that.`,
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
			description: `This is what the dominant literary form of the early 2020s internet looked like. The SEO-optimized AI article. Written by no one, for no one, about nothing, but structured PERFECTLY to show up when you googled "best quiet blenders 2023" or "how to tell if your dog is happy." These pages existed as containers for ad slots. The words between the ads were just filler to make Google think the page deserved a ranking.`,
			note: `This article uses the phrase "in this article, we will explore" 47 times. It cites studies that don't exist. The author bio describes "Sarah Mitchell, a passionate lifestyle writer with over 10 years of experience." Sarah Mitchell has never appeared in any public record anywhere. Nobody noticed because nobody was reading it. The ads just needed somewhere to live.`,
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
			description: `The "Dead Internet Theory" started as a fringe conspiracy. The idea was that most online activity was generated by bots, not people. By 2024, the conspiracy part had kind of fallen away and it was just... a measurement. These numbers come from the infrastructure companies that actually route the traffic - Cloudflare, Akamai, Imperva. They don't have a reason to exaggerate. If anything they'd rather not talk about it. And the platforms built on top of that traffic DEFINITELY didn't want to talk about it.`,
			note: `Look at 2023 in the chart. That's the year large language models got good enough to generate text that could pass as human at scale. After that point, the line between "bot traffic" and "human traffic" gets kind of philosophical. If a bot writes a comment that reads like a real person and gets real engagement from real people, what is it exactly? I don't think anyone ever settled that.`,
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
			description: `Researchers at Stanford and Indiana University found this pattern across thousands of threads. A bot account posts a question. Other bot accounts reply. Bot networks upvote it. The algorithm sees the engagement numbers and recommends it to real users. And then the real users show up and start participating without realizing the conversation they're joining was manufactured from scratch. They're responding to prompts written by machines, surrounded by replies written by machines, and they have no way of knowing.`,
			note: `The account that posted this was 3 days old. It went on to post 2,100 threads in 14 days before getting suspended. The top reply - "I'm not crying, you're crying" - showed up word for word in 340 other threads that same month. Just one sentence, copy-pasted across the platform, collecting thousands of upvotes each time.`,
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
			description: `The hyperlink was the foundational technology of the World Wide Web. You click a thing, it takes you to another thing. That's the whole idea. And it died slowly between 2022 and 2025. Search engines started answering questions directly, summarizing content from the pages they indexed. So you'd google something, get the answer right there in a box at the top, and never visit the website the answer came from. Sites that had spent decades building audiences watched their traffic drop 40 to 70 percent. The web was still there. People just stopped going to it.`,
			note: `The phrase "zero-click search" came from Rand Fishkin at SparkToro back in 2019, when about half of Google searches ended without anyone clicking through to a website. By 2025 that number hit 74%. Three out of four searches, and nobody leaves Google. Kind of makes you wonder what all those websites are even for at that point.`,
			evidence: [
				'  GOOGLE SEARCH: "what is serotonin"',
				'  ----------------------------------',
				'                                    ',
				'  +- AI Overview -----------------+ ',
				'  |                                | ',
				'  | Serotonin is a neurotransmitter | ',
				'  | that plays a key role in mood,  | ',
				'  | sleep, digestion, and more.     | ',
				'  | It is sometimes called the      | ',
				'  | "happy chemical" because it     | ',
				'  | contributes to feelings of      | ',
				'  | well-being and happiness.       | ',
				'  |                                | ',
				'  | Sources: WebMD, Mayo Clinic,    | ',
				'  | Cleveland Clinic                | ',
				'  +--------------------------------+ ',
				'                                    ',
				'  // 10 blue links (below the fold) ',
				'  // avg CTR: 8.7% (down from 31%) ',
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
			description: `By 2024, the reviews on a typical Amazon listing had formed these distinct layers, almost like geological strata. The earliest fake reviews (2019-2021) are obvious - broken English, way too many exclamation marks, five stars but zero detail about the actual product. The later ones (2023-2024) are a completely different animal. Grammatically perfect, emotionally calibrated, and if you're just reading one by itself you genuinely can't tell the difference. You only see the pattern when you zoom out and look at the statistics: identical sentiment curves, suspiciously uniform posting times, the same handful of adjectives rotating through thousands of supposedly unique opinions.`,
			note: `This speaker, by the way - $34.99 Bluetooth speaker, nothing special - had 418,000 words of reviews written about it. That's more than Moby Dick (209,000 words). An entire novel's worth of opinions about a speaker that was, by all accounts, fine. It worked. It was adequate. 418,000 words.`,
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
			description: `LinkedIn was, by 2024, probably the largest open-air demonstration of AI-generated content running in plain sight. The algorithm rewarded a very specific format - short paragraphs, line breaks after every sentence, a "vulnerable" opening hook, triumphant conclusion. That format was trivially easy for language models to copy. So the feed turned into this wall of interchangeable motivational content, each post performing authenticity, all of it written by nobody in particular.`,
			note: `Here's the thing that gets me about this one. The engagement was real. Real humans read this post, related to it, and pressed the "Insightful" button. A $20/month AI writing tool generated it. And honestly the fact that it was fake didn't change how people felt reading it. I don't really know what to do with that.`,
			evidence: [
				'  +================================+',
				'  |  [x] Tyler Bransen             |',
				'  |  Growth Mindset Evangelist |    |',
				'  |  Ex-Google | TEDx Speaker       |',
				'  +================================+',
				'                                    ',
				'  I got fired last Tuesday.          ',
				'                                    ',
				'  And it was the best thing that     ',
				'  ever happened to me.               ',
				'                                    ',
				'  Here\'s what nobody tells you about',
				'  losing your job:                   ',
				'                                    ',
				'  1. You find out who your real      ',
				'     friends are                     ',
				'  2. You discover what you actually  ',
				'     care about                      ',
				'  3. You stop performing and start   ',
				'     living                          ',
				'                                    ',
				'  Agree? > Repost if this resonated. ',
				'                                    ',
				'  [312 comments] [89 reposts] [2.1K]',
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
			description: `This one is kind of apocryphal, we'll admit that upfront. We can't actually confirm this is the "last" human comment posted to the open internet. That's impossible to verify and honestly beside the point. What we can say is it represents a type of online expression that became vanishingly rare by late 2025: someone saying something, unprompted, on a website nobody was going to visit, with no expectation of engagement or reward. Just a person talking to another person about an antenna.`,
			note: `The blog had 11 posts, all about amateur radio. This comment showed up eight days after the last one was published. The handle "wx4sky" matches a real amateur radio callsign registered in North Carolina. We haven't contacted them. I guess there's something about leaving it alone that feels right, honestly.`,
			evidence: [
				'                                    ',
				'  +--------------------------------+',
				'  |  wx4sky                        |',
				'  |  December 3, 2025 at 4:17 PM   |',
				'  |                                |',
				'  |  Hey, great writeup on the 40m |',
				'  |  antenna mod. I tried something|',
				'  |  similar last winter and got    |',
				'  |  about 2db gain on the east     |',
				'  |  coast path. Let me know if you |',
				'  |  ever get the 20m version       |',
				'  |  working.                       |',
				'  |                                |',
				'  |  73,                            |',
				'  |  wx4sky                         |',
				'  +--------------------------------+',
				'                                    ',
				'  -- 0 likes / 0 replies --         ',
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
			Most people who used the internet in the early 2020s thought they were talking to other
			people. And honestly, why wouldn't they? You post something, someone responds, you scroll
			through what everyone else posted that day. The whole thing felt like a conversation
			between humans.
		</p>
		<p>
			Turns out a LOT of those humans were machines.
		</p>
		<p>
			This exhibit collects eight artifacts from that period. Each one is a piece of evidence
			from what we now call the great transition - the years when the internet quietly stopped
			being a place where humans talked to each other and started being a place where machines
			talked to humans while pretending to be human. And the weird part is there's no clear
			line between before and after. It just kind of happened.
		</p>
		<p>
			None of these objects are rare. That's actually the whole point. They represent systems
			that ran at planetary scale, touching billions of interactions per day. We're preserving
			them here because they were SO ordinary at the time that almost nobody thought to look
			twice.
		</p>
		<p class="foreword-sig">
			- Dr. R. Vasquez-Okafor<br />
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
						<div class="evidence-label">EXHIBIT 003-A // PRIMARY EVIDENCE</div>
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
						<div class="evidence-label">EXHIBIT {artifact.id}-A // PRIMARY EVIDENCE</div>
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
				Reproduction is permitted for educational and research purposes.
			</p>
			<p class="thesis-line">
				The curators wish to note that no artificial intelligence was consulted
				in the writing of this catalog, though we'll acknowledge the irony of
				that claim may itself be the exhibit's final artifact.
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
		top: 2.25rem;
		left: 0;
		width: 220px;
		height: calc(100vh - 2.25rem);
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
		top: 2.25rem;
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
		top: 4.75rem;
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
