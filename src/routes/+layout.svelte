<script>
	import Header from './Header.svelte';
	import '../app.css';
	import Page from './+page.svelte';

	let { children } = $props();
</script>

<div class="app">
	<Header />

	<main>
		{@render children()}
	</main>

	<footer>
		<div class="max-w-3xl mx-auto p-4 border border-gray-700 rounded-lg">
    		<div class="text-column">
            	<p><strong>Methodology</strong></p>
				<p>This quiz was adapted from Figure 3 in <a href="https://www.bmj.com/content/370/bmj.m3223#F3" target="_blank">Jones N R, Qureshi Z U, Temple R J, Larwood J P J, Greenhalgh T, Bourouiba L et al. Two metres or one: what is the evidence for physical distancing in covid-19? <em>BMJ</em> 2020;370:m3223</a>.</p>
				<p>I calculated all the possible valid answer combinations for the quiz — of which there were 72 — and then manually matched each combination with its corresponding risk level — low, medium or high — as given in Figure 3. The two combinations that were marked as borderline cases between low and medium risk have been shown here as low to medium risk.</p>
				<p>The header image is 'Visualization of the Coronavirus' by Fusion Medical Animation on <a href="https://unsplash.com/photos/visualization-of-the-coronavirus-EAgGqOiDDMg" target="_blank">Unsplash</a>.</p>
				<p>You can view the code behind this story in the <a href="https://github.com/ZtheTrain/COVID-transmission-risk" target="_blank"> GitHub repository</a>.</p>
        	</div>
		</div>
	</footer>
</div>

<style>
	.app {
		display: flex;
		flex-direction: column;
		min-height: 100vh;
	}

	main {
		flex: 1;
		display: flex;
		flex-direction: column;
		padding: 1rem;
		width: 100%;
		max-width: 64rem;
		margin: 0 auto;
		box-sizing: border-box;
	}

	footer {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding: 6px;
	}

	p {
		line-height: 1.5;
        margin: 0;
        padding-bottom: 1.2em;
	}

	@media (min-width: 480px) {
		footer {
			padding: 12px 0;
		}
	}
</style>
