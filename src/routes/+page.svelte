<script>
	import Counter from '$lib/components/Counter.svelte';
	import welcome from '$lib/images/svelte-welcome.webp';
	import welcome_fallback from '$lib/images/svelte-welcome.png';
</script>

<svelte:head>
	<title>Home</title>
	<meta name="description" content="Svelte demo app" />
</svelte:head>

<section class="flow mx-auto text-center">
	<span>
		<picture>
			<source srcset={welcome} type="image/webp" />
			<img src={welcome_fallback} alt="Welcome" />
		</picture>
	</span>
	<h1 class="heading-1 mx-auto">Welcome</h1>
	<div class="flex-group mx-auto">
		<Counter />
	</div>
</section>
