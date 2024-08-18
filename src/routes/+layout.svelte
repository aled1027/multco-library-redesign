<script>
	import Header from '$lib/components/Header.svelte';
	import '../styles/main.scss';
</script>

<div class="app">
	<Header />
	<main>
		<slot />
	</main>
</div>
