<script lang="ts">
</script>

<div class="container search-hero | flow" data-type="full-width">
	<div class="search-area">
		<form class="form" aria-labelledby="search-label">
			<div class="search-hero-background">
				<svg width="100" height="100" viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
					<circle cx="50" cy="50" r="40" stroke="black" stroke-width="5" fill="none">
						<animate
							attributeName="r"
							from="40"
							to="20"
							dur="10s"
							begin="0s"
							repeatCount="indefinite"
						/>
						<animate
							attributeName="stroke-opacity"
							from="1"
							to="0"
							dur="10s"
							begin="0s"
							repeatCount="indefinite"
						/>
					</circle>
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
	<div class="box">1</div>
	<div class="box">2</div>
	<div class="box">3</div>
	<div class="box">4</div>
</div>
