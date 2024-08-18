<script lang="ts">
	// import gsap from 'gsap';
	import KUTE from 'kute.js';
	import { onMount } from 'svelte';
	let rows = 4;
	let cols = 4;

	function runFlagAnimation() {
		// This is the animation I really like from gsap
		// somewhat reproduced with KUTE.js but not as good imo

		// gsap
		// 	.timeline()
		// 	.from('.flag-rect1', {
		// 		duration: 0.5,
		// 		opacity: 0
		// 	})
		// 	.from('path:nth-child(2)', {
		// 		duration: 1,
		// 		scale: 0,
		// 		transformOrigin: 'center',
		// 		ease: 'elastic.out(1, 0.75)'
		// 	})
		// 	.from('path:nth-child(3)', {
		// 		duration: 2,
		// 		drawSVG: 0,
		// 		ease: 'power1.inOut'
		// 	})
		// 	.from('path:nth-child(4)', {
		// 		duration: 1,
		// 		scale: 0,
		// 		transformOrigin: 'center',
		// 		ease: 'elastic.out(1, 0.75)'
		// 	});

		// Reset the state of the elements before starting the animation
		document.getElementById('pdx-flag-green').style.transform = 'scale(0)';
		document.getElementById('pdx-flag-blue').style.transform = 'scale(0)';
		document.getElementById('pdx-flag-yellow').style.opacity = '0';
		document.getElementById('pdx-flag-bg').style.color = '#324FA4';
		document.getElementById('svg-inner-container').style.transform = 'translateX(0%)';

		KUTE.fromTo(
			'#pdx-flag-green',
			{ scale: 0 }, // Initial state
			{ scale: 1 }, // Final state
			{
				duration: 1000, // Duration in milliseconds (1 second)
				easing: 'linear', // Similar to GSAP's elastic.out
				transformOrigin: 'center' // Set transform origin to center
			}
		).start();

		KUTE.fromTo(
			'#pdx-flag-yellow',
			{ draw: '0% 0%', opacity: 1 },
			{ draw: '0% 100%', opacity: 1 },
			{ duration: 4500, delay: 1000, easing: 'easingCubicInOut' }
		).start();

		KUTE.fromTo(
			'#pdx-flag-blue',
			{ scale: 0 },
			{ scale: 1 },
			{
				duration: 1000,
				delay: 3200,
				easing: 'easingCubicInOut',
				transformOrigin: 'center'
			}
		).start();

		KUTE.fromTo(
			'#pdx-flag-bg',
			{ color: '#324FA4' },
			{ color: '#fff' },
			{
				duration: 4000,
				easing: 'easingCubicIn'
			}
		).start();

		KUTE.fromTo(
			'#svg-inner-container',
			{ translateX: '0%' }, // Start state (original position)
			{ translateX: '-1000%' }, // End state (moved out of the viewport to the left)
			{ duration: 1500, delay: 8000, easing: 'easingCubicIn' } // Animation options
		).start();
	}

	onMount(() => {
		runFlagAnimation();
		setInterval(runFlagAnimation, 20000);
	});
</script>

<div class="container search-hero | flow" data-type="full-width">
	<div class="search-area">
		<form class="form" aria-labelledby="search-label">
			<div class="search-hero-background">
				<svg
					id="flag-svg"
					class="top-middle-svg"
					xmlns="http://www.w3.org/2000/svg"
					viewbox="0 0 600 360"
				>
					<g id="svg-inner-container">
						<rect id="pdx-flag-bg" width="600" fill="currentColor" height="360" />
						<path
							id="pdx-flag-green"
							style="transform: scale(0);"
							fill="#046a38"
							d="m350 215h250v145h-250zm-350 50h230v95h-230zm300-265h300v95h-300zm-300 0h180v145h-180z"
						/>
						<path
							id="pdx-flag-yellow"
							style="opacity: 0;"
							fill="none"
							stroke="#ffb81c"
							stroke-width="20"
							d="m200 0v165h-200m280-165v115h320m-600 130h250v115m350-165h-270v165"
						/>
						<path
							id="pdx-flag-blue"
							style="transform: scale(0);"
							fill="#418fde"
							d="m310 360v-175a40 40 0 0 0 -40 40v135zm-310-135h260a40 40 0 0 0 -40 -40h-220zm600-90h-330a40 40 0 0 0 40 40h290zm-380-135v175a40 40 0 0 0 40 -40v-135z"
						/>
					</g>
				</svg>

				<svg class="bottom-left-svg" viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
					<g transform="translate(20,20)">
						{#each Array(rows).fill(0) as _, row}
							{#each Array(cols).fill(0) as _, col}
								<circle cx={col * 40} cy={row * 40} r="8" fill="black" opacity="0">
									<animate
										attributeName="opacity"
										values="0; 1; 0"
										keyTimes="0; 0.5; 1"
										dur="7s"
										begin={`${0.1 * (row * cols + col)}s`}
										repeatCount="indefinite"
									/>
								</circle>
							{/each}
						{/each}
					</g>
				</svg>
			</div>
			<label for="search" id="search-label" class="label">What do you want to find:</label>
			<div class="input-wrapper">
				<input
					id="search"
					name="search"
					type="text"
					placeholder="Search term"
					aria-label="Search term"
				/>
				<button type="submit" aria-label="Submit search">
					<svg viewBox="0 0 1200 1200" xmlns="http://www.w3.org/2000/svg">
						<title>Submit search</title>
						<path
							d="M712.78 142.78 597.56 259.17l259.22 259.22H30v163.22h826.78L597.6 940.79l115.22 116.39L1170 600z"
						/>
					</svg>
				</button>
			</div>
		</form>
	</div>
	<div class="box">
		<a class="heading-3" href="#">Books</a>
	</div>
	<div class="box">
		<a class="heading-3" href="#">Events</a>
	</div>
	<div class="box">
		<a class="heading-3" href="#">Locations</a>
	</div>
	<div class="box">
		<a class="heading-3" href="#">Other Services</a>
	</div>
</div>
