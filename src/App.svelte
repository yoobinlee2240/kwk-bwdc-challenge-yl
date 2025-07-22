<script>
	import { onMount } from 'svelte';
	import { fade } from 'svelte/transition';

	let showViz = false;
	let scrollPercent = 0;

	let title = "If you were a DEI program reciter in tech...";
	let subtitle = "by Yoobin Lee";
	let fact = "Black people with STEM degrees have the lowest employment rate in STEM among all racial groups.";
	let source = "Black Wealth Data Center";

	onMount(() => {
		const observer = new IntersectionObserver(
			(entries) => {
				entries.forEach((entry) => {
					if (entry.isIntersecting) {
						showViz = true;
						observer.disconnect();

						setTimeout(() => {
							const divElement = document.getElementById("viz1753151734489");
							const vizElement = divElement?.getElementsByTagName("object")[0];
							if (vizElement) {
								vizElement.style.width = "100%";
								vizElement.style.height = "727px";
								const scriptElement = document.createElement("script");
								scriptElement.src = "https://public.tableau.com/javascripts/api/viz_v1.js";
								vizElement.parentNode.insertBefore(scriptElement, vizElement);
							}
						}, 200);
					}
				});
			},
			{ threshold: 0.3 }
		);

		const trigger = document.getElementById("viz-trigger");
		if (trigger) observer.observe(trigger);
	});

	function handleScroll() {
		const scrollTop = window.scrollY;
		const docHeight = document.documentElement.scrollHeight - window.innerHeight;
		scrollPercent = Math.min(100, Math.round((scrollTop / docHeight) * 100));
	}
</script>

<svelte:window on:scroll={handleScroll} />
<div class="progress-bar" style="width: {scrollPercent}%"></div>

<main>
	<header>
		<h1>{title}</h1>
		<h2>{subtitle}</h2>
	</header>

	<section class="intro">
		<p>
			The tech and STEM industry continues to grow at a rapid pace. However, according to the <em>{source}</em>, Black individuals with STEM degrees have the lowest employment rates in STEM fields compared to Asian, White, and Hispanic groups.
		</p>
	</section>

	<div id="viz-trigger" style="height: 1px;"></div>

	{#if showViz}
		<section class="viz" in:fade={{ duration: 800 }}>
			<h3>STEM Employment by Race/Ethnicity</h3>
			<div class="tableauPlaceholder" id="viz1753151734489" style="position: relative">
				<noscript>
					<a href="#">
						<img
							alt="Percentage Distribution with a Bachelor's Degree by Occupation and Race/Ethnicity"
							src="https://public.tableau.com/static/images/BW/BWDC-PersonLevel-V1/STEMEmployment/1_rss.png"
							style="border: none"
						/>
					</a>
				</noscript>
				<object class="tableauViz" style="display:none;">
					<param name="host_url" value="https%3A%2F%2Fpublic.tableau.com%2F" />
					<param name="embed_code_version" value="3" />
					<param name="site_root" value="" />
					<param name="name" value="BWDC-PersonLevel-V1/STEMEmployment" />
					<param name="tabs" value="no" />
					<param name="toolbar" value="yes" />
					<param name="static_image" value="https://public.tableau.com/static/images/BW/BWDC-PersonLevel-V1/STEMEmployment/1.png" />
					<param name="animate_transition" value="yes" />
					<param name="display_static_image" value="yes" />
					<param name="display_spinner" value="yes" />
					<param name="display_overlay" value="yes" />
					<param name="display_count" value="yes" />
					<param name="language" value="en-US" />
					<param name="origin" value="viz_share_link" />
				</object>
			</div>
		</section>
	{/if}

	<section class="intro">
		<p>
			This data reveals a critical opportunity: if tech companies genuinely want to strengthen their DEI efforts, they must prioritize outreach to and support for Black STEM graduates.
		</p>
	</section>

	<section class="fact">
		<p class="stat">"{fact}"</p>
		<cite class="source">— {source}</cite>
	</section>
</main>

<style>
	.progress-bar {
		position: fixed;
		top: 0;
		left: 0;
		height: 5px;
		background-color: #d43a3a;
		z-index: 999;
		transition: width 0.15s ease-out;
	}

	main {
		font-family: system-ui, sans-serif;
		max-width: 850px;
		margin: 3rem auto;
		padding: 1.5rem;
		color: #222;
	}

	header {
		text-align: center;
		margin-bottom: 2.5rem;
	}

	h1 {
		font-size: 2rem;
		color: #2c2c2c;
		margin-bottom: 0.5rem;
	}

	h2 {
		color: #555;
		font-weight: 400;
		font-size: 1.1rem;
		margin-bottom: 1rem;
	}

	.intro p {
		margin-bottom: 1rem;
		font-size: 1.05rem;
	}

	.fact {
		margin-top: 2rem;
		background: #fef6f6;
		padding: 1rem 1.25rem;
		border-left: 4px solid #d43a3a;
	}

	.stat {
		font-weight: 600;
		font-size: 1.1rem;
		margin-bottom: 0.5rem;
	}

	.source {
		font-size: 0.9rem;
		color: #666;
		font-style: normal;
	}

	.viz {
		margin-top: 3rem;
	}

	.viz h3 {
		font-size: 1.25rem;
		margin-bottom: 1rem;
		text-align: center;
	}

	.tableauPlaceholder {
		width: 100%;
		overflow: hidden;
	}
</style>
