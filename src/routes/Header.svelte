<script>
	import { page } from '$app/state'

</script>

<header>
	<div class="header with-image-bg">
        <div class="text-column">
            <h1>Catching COVID</h1>
            <p class="subhead">During the first year of the COVID-19 pandemic, scientists raced to understand how the virus was being transmitted.</p>
        </div>
	</div>
</header>

<style>
	header {
		display: flex;
		/* justify-content: center; */
	}

	/* Ensures .with-image-bg takes up full width */
	.with-image-bg {
		display: block;
		position: relative;
		background-size: cover;
		background-position: center; /* Optional, to center the background image */
		margin-bottom: 2em;

		/* Full width of the viewport */
		width: 100vw; /* Takes up full viewport width */
		top: 0;
		left: 50%; /* Adjust positioning to ensure it spans full width */
		right: 50%;
		margin-left: -50vw; /* Offset by half the viewport width to center it */
		margin-right: -50vw; /* Same for the right side */

		/* Center everything inside */
		min-height: 70svh;
		display: flex;
		justify-content: center;
		align-items: center;

		/* White text with a shadow */
		color: white;
		text-shadow: 0px 0px 30px black,
			0px 0px 10px rgba(0, 0, 0, 0.5);
		
		background-image: url('/fusion-medical-animation-EAgGqOiDDMg-unsplash.jpg');
	}

	h1 {
        width: 100%;
		font-family: 'Lora', Georgia, serif;
        font-weight: bold;
        font-size: 4em;
        line-height: 1.5;
    }

    .subhead {
		font-weight: bold;         /* Keep the bold text */
		font-size: 1.5em;          /* Set the font size */
		text-align: center;       /* Justify the text across the container */
		line-height: 1.5;          /* Optional: improve readability with better line height */
		margin: 0 auto;            /* Center the paragraph block horizontally (useful if it's a block element) */
	}

</style>
