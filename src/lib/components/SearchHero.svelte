<script lang="ts">
	let rows = 4;
	let cols = 4;
</script>

<div class="container search-hero | flow" data-type="full-width">
	<div class="search-area">
		<form class="form" aria-labelledby="search-label">
			<div class="search-hero-background">
				<svg viewBox="0 0 200 200" xmlns="http://www.w3.org/2000/svg">
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
