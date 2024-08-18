<script lang="ts">
	let rows = 4;
	let cols = 4;
</script>

<div class="container hero | flow" data-type="full-width">
	<div class="left-area">
		<div class="hero-background">
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
		<p class="text">An unofficial resource for</p>
		<p class="text">events at the Multnomah County Library</p>
	</div>
	<a href="/" class="box | bg-primary-300">Family Storytime</a>
	<a href="/" class="box | bg-purple-300">Events Today</a>
	<a href="/" class="box | bg-accent-300">Tech Help</a>
	<a href="/" class="box | bg-green-300">All Events</a>
</div>
