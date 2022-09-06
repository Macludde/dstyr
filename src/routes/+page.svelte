<script lang="ts">
	import ModelShowcase from '$lib/features/ModelShowcase.svelte';
</script>

<svelte:head>
	<title>D Styr</title>
	<meta name="description" content="D som styr D-sektionen" />
</svelte:head>

<section>
	<div class="container">
		<h1 class="title">Vem styr? <a href="/">D Styr</a>!</h1>
		<ModelShowcase />
	</div>
</section>

<style>
	.title {
		text-align: center;
		font-size: 4rem;
		/* Prevent margin bleeding at top of page */
		margin-block-start: 0;
		padding-block-start: 0.83em;
	}
</style>
