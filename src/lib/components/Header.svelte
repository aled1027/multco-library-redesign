<script>
	let dialogRef;

	function openDialog() {
		dialogRef.showModal();
	}

	function closeDialog() {
		dialogRef.close();
	}
</script>

<header class="site-header">
	<div class="site-header__inner">
		<div class="title-container">
			<a href="/">Multco Library Events</a>
			<span class="subtitle">An unofficial resource of Multnomah County library events</span>
		</div>
		<button class="menu-button" on:click={openDialog}>About</button>
	</div>
	<dialog class="" bind:this={dialogRef}>
		<button class="close-button" on:click={closeDialog}>&times;</button>
		<div class="flow">
			<h2 class="heading-3">About this site</h2>
			<p>
				This is a demo and experiment for displaying events for the Multnomah county library. The
				events are sourced from
				<a href="https://multcolib.org/events-classes">https://multcolib.org/events-classes</a>.
			</p>
			<p>
				At times, I've had issues navigating the events at the library, and some pages on the
				library website load slowly, are not aesthetically pleasing, and struggle with some
				lighthouse metrics, so this is me exploring ways to address those issues.
			</p>
			<p>
				Please note that this is a demo. A full implementation as needed by the library would have
				additional requirements, such as internationalization and language support.
			</p>

			<p>
				A major influence for the design of this website is the <a href="https://www.berlin.de/en/">
					city of Berlin website</a
				>.
			</p>
			<p>
				Made by <a href="https://alexledger.net">Alex Ledger</a>.
			</p>
		</div>
	</dialog>
</header>

<style>
	dialog {
		position: absolute;
		inset: 0;
		margin: auto;
		border: none;
		border-radius: 8px;
		padding: 1.5rem;
		max-width: 500px;
		width: 100%;
		height: fit-content;
		box-shadow: 0 2px 10px rgba(0, 0, 0, 0.2);
	}

	dialog::backdrop {
		background-color: rgba(0, 0, 0, 0.5);
	}

	.close-button {
		background: none;
		border: none;
		font-size: 4rem;
		position: absolute;
		top: 6px;
		right: 6px;
		cursor: pointer;

		height: 4rem;
		width: 4rem;
		border-radius: 50%;
		display: grid;
		place-content: center;
	}

	.close-button:hover {
		background-color: rgba(0, 0, 0, 0.1);
	}
</style>
