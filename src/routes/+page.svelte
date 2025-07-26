<script>
	import { writable } from 'svelte/store';

	// Writable stores for each question
	let setting = writable(''); // For Q1: "Where were you?"
	let people = writable(''); // For Q2: "How many people were there?"
	let mask = writable(''); // For Q3: "Were you wearing masks?"
	let contact = writable(''); // For Q4: "How long were you in contact?"
	let activity = writable(''); // For Q5: "Were you speaking?"

	// Store to hold the result of risk level
	let risk = writable('');

	// Variable to hold the risk data (from the JSON file)
	let riskData = [];

	// Fetch risk data when the component mounts
	import { onMount } from 'svelte';

	onMount(async () => {
		const res = await fetch('risk_data.json');
		if (res.ok) {
			riskData = await res.json();
		} else {
			console.error("Failed to load risk data");
		}
	});

	// Function to get the combined answers with checks for empty values
	function getCombinedAnswers() {
		const answers = [
			$setting || 'Not selected', 
			$people || 'Not selected', 
			$mask || 'Not selected', 
			$contact || 'Not selected', 
			$activity || 'Not selected'
		];
    
    return answers.join(' + ');
}

	// Function to get the risk based on the selected combination
	function getRiskLevel(combination) {
    	return riskData[combination] || "Unknown Risk";
	}

	// Function to trigger when 'Calculate' is clicked
	function calculate() {
    // Validation step: Check if any answer is missing
    	if ($setting === '' || $people === '' || $mask === '' || $contact === '' || $activity === '') {
        	document.getElementById('error-message').classList.remove('hidden'); // Show error message
        	return; // Stop the function if any question is unanswered
    	}

    // Hide error message if all questions are answered
    	document.getElementById('error-message').classList.add('hidden');

       	const combination = getCombinedAnswers();
    	const riskLevel = getRiskLevel(combination);
    	risk.set(riskLevel); // Update the risk level store
	  	console.log("Risk Level: ", riskLevel); // Print the risk level in the console
}
</script>

<svelte:head>
	<title>Catching COVID</title>
	<meta name="description" content="COVID risk app" />
</svelte:head>

<div class="text-column">
	
	<p class="byline">
		By <a href="https://www.suzannahlyons.com" target="_blank">Suzannah Lyons</a>
	</p>
	<p>
		In early 2020 I was <a href="https://www.abc.net.au/news/suzannah-lyons/6023890" target="_blank">a science reporter</a> for the Australian Broadcasting Corporation, more concerned with <a href="https://www.abc.net.au/news/science/2020-01-11/keeping-your-home-smoke-free/11857898" target="_blank">reporting on the impacts of Australia's Black Summer bushfire crisis</a> than the news coming out of China of <a href="https://www.abc.net.au/news/health/2020-01-20/coronavirus-chinese-pneumonia-mystery-explainer/11882490" target="_blank">a new type of viral pneumonia</a>.
	</p>
	<p>
		That soon changed and after the World Health Organization <a href="https://www.who.int/director-general/speeches/detail/who-director-general-s-opening-remarks-at-the-media-briefing-on-covid-19---11-march-2020" target="_blank">declared COVID-19 a pandemic</a> on March 11, 2020, <a href="https://www.abc.net.au/news/redirects/backstory/2020-07-03/how-abc-science-is-reporting-on-coronavirus-covid19/12321990" target="_blank">like many of my colleagues</a> I was reporting on little else.
	</p>
	<p>
		In reflecting on that time, a few things stand out to me now.
	</p>
	<p>
		Usually as a science journalist you're reporting on research once it's been finished, thoroughly peer-reviewed and published — a process that in some cases can take years.
	</p>
	<p>
		But during the pandemic it felt more like we were seeing the science around SARS-CoV-2 and COVID develop in real time, speaking to researchers regularly about what they could tell us from the latest research, what was speculation within their area of expertise and what they just didn't know yet.
	</p>
	<p>
		We did this so we could write stories for an audience desperate for information — towards the end of September 2020 the ABC had received <a href="https://www.abc.net.au/news/redirects/backstory/2020-09-20/how-abc-news-answered-coronavirus-covid19-questions/12624260" target="_blank">more than 100,000 questions</a> from the public — and as the saying goes 'news they could use'.
	</p>
	<p>
		Despite the <a href="https://www.ncbi.nlm.nih.gov/research/coronavirus/" target="_blank">deluge of research</a> being produced during that time, you would sometimes come across papers that really stood out as being hugely important in taking our knowledge of COVID forward.
	</p>
	<p>
		This story was inspired by one of them.
	</p>
	<p>
		On August 25, 2020 the <em>British Medical Journal</em> published an analysis entitled <a href="https://www.bmj.com/content/370/bmj.m3223" target="_blank">'Two metres or one: what is the evidence for physical distancing in covid-19?'</a>.
	</p>
	<p>
		In the paper, the authors argued that our understanding of how far we should physically distance ourselves from other people to reduce our risk of catching COVID was based on outdated science.
	</p>
	<p>
		Instead, they said we should be looking at multiple factors that influence this risk.
	</p>
	<p>
		As part of their work, the scientists produced a summary figure detailing the relative risk of COVID transmission under different conditions.
	</p>
	<p>
		When I first saw that figure I wanted to turn it into an interactive quiz to make it more accessible to a general public audience. I haven't had the skills to do so until now.
	</p>
	<div class="text-center">
	  <div class="border-t border-gray-700 w-1/2 my-4"></div>
	</div>
	<p>
		So take yourself back to 2020 and hearing the news you attended an event with someone who has since tested positive for COVID.
	</p>
	<p>
	Is your risk of catching COVID from that person low, medium or high? Let's find out.
	</p>
</div>

<section>
	<div class="max-w-lg mx-auto p-6 bg-white shadow-lg rounded-lg border border-gray-200 mt-6 mb-6">

		<h2 class="text-2xl font-bold mb-4"><strong>Calculate your risk of catching COVID from someone not showing symptoms</strong></h2>
		
		<!-- Question 1 -->
		<div class="mb-4">
			<p class="text-lg font-medium"><strong>1. Where were you?</strong></p>

			<div class="mt-2 space-y-2">
			{#each [
				'Somewhere outdoors and well ventilated', 
				'Somewhere indoors and well ventilated', 
				'Somewhere poorly ventilated'
				] as location}
				<div>
					<label class="block">
						<input type="radio" name="setting" class="mr-2 leading-tight" bind:group={$setting} value={location} required />
						<span class="text-gray-700">{location}</span>
					</label>
				</div>
			{/each}
			</div>

		</div>

		<!-- Question 2 -->
		<div class="mb-4">
			<p class="text-lg font-medium"><strong>2. How many people were there?</strong></p>

			<div class="mt-2 space-y-2">
			{#each [
				'There were not many people there', 
				'There were lots of people there'
				] as occupancy}
				<div>
					<label class="block">
						<input type="radio" name="people" class="mr-2 leading-tight" bind:group={$people} value={occupancy} required />
						<span class="text-gray-700">{occupancy}</span>
					</label>
				</div>
			{/each}
			</div>

		</div>

		<!-- Question 3 -->
		<div class="mb-4">
			<p class="text-lg font-medium"><strong>3. Were you wearing face masks?</strong></p>

			<div class="mt-2 space-y-2">
			{#each [
				'We were wearing masks', 
				'We weren\'t wearing masks'
				] as maskOption}
				<div>
					<label class="block">
						<input type="radio" name="mask" class="mr-2 leading-tight" bind:group={$mask} value={maskOption} required />
						<span class="text-gray-700">{maskOption}</span>
					</label>
				</div>
			{/each}
			</div>

		</div>

		<!-- Question 4 -->
		<div class="mb-4">
			<p class="text-lg font-medium"><strong>4. How long were you in contact with the person who tested positive to COVID?</strong></p>

			<div class="mt-2 space-y-2">
			{#each [
				'For a short time', 
				'For a prolonged time'
				] as contactOption}
				<div>
					<label class="block">
						<input type="radio" name="contact" class="mr-2 leading-tight" bind:group={$contact} value={contactOption} required />
						<span class="text-gray-700">{contactOption}</span>
					</label>
				</div>
			{/each}
			</div>

		</div>

		<!-- Question 5 -->
		<div class="mb-4">
			<p class="text-lg font-medium"><strong>5. Were you speaking?</strong></p>

			<div class="mt-2 space-y-2">
			{#each [
				'We were silent', 
				'We were speaking', 
				'We were shouting or singing'
				] as activityOption}
				<div>
					<label class="block">
						<input type="radio" name="activity" class="mr-2 leading-tight" bind:group={$activity} value={activityOption} required />
						<span class="text-gray-700">{activityOption}</span>
					</label>
				</div>
			{/each}
			</div>

		</div>

		<!-- Calculate Button -->
		<div class="text-center p-4">
			<button class="bg-blue-500 text-white px-4 py-2 rounded-full hover:bg-blue-600 focus:outline-none" on:click={calculate}>Calculate your risk</button>
		</div>
		
		<div id="error-message" class="mb-4 text-red-500 text-center hidden"><strong>Please answer all questions before calculating your risk.</strong></div>

		<!-- This will be hidden until the risk score is revealed -->
		{#if $risk}
  		<div class="mt-4 text-center p-4">
    		<p class="text-xl font-semibold">
		      You had a <strong class="text-xl text-amber-600">{ $risk }</strong> of catching COVID-19.
    		</p>
  		</div>

		<div class="mt-4">
			<p class="text-sm">Change your answers and calculate your risk again to see how it varied under different conditions.</p>
		</div>
		{/if}

		<div class="mt-4">
			<hr class="border-t border-gray-300">
			<p class="text-xs mt-2">Interactive: Suzannah Lyons <span>&middot;</span> Adapted from Figure 3 in <a href="https://www.bmj.com/content/370/bmj.m3223#F3" target="_blank">Jones N R, Qureshi Z U, Temple R J, Larwood J P J, Greenhalgh T, Bourouiba L et al. Two metres or one: what is the evidence for physical distancing in covid-19? <em>BMJ</em> 2020;370:m3223</a>. This quiz provides general information only of your relative risk of catching COVID-19 from someone who was asymptomatic during the height of the COVID pandemic. You may need to also consider other factors, depending on your personal circumstances.</p>
		</div>

	</div>
</section>

<div class="text-column">
	<p>
		Although we're no longer in the midst of a pandemic, according to the WHO there remains <a href="https://www.who.int/emergencies/disease-outbreak-news/item/2025-DON572" target="_blank">a high global public health risk</a> from COVID. So it's still useful to know what factors increase your risk of catching COVID from someone who's not showing any symptoms.
	</p>
</div>

<style>
	section {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		flex: 0.6;
	}

	p {
		line-height: 1.5;
        margin: 0;
        padding-bottom: 1.2em;
	}

</style>