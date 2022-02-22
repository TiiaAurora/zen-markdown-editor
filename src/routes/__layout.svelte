<script lang="ts">
	import Footer from '$lib/Footer.svelte';

	import '../app.css';
</script>

<main>
	<slot />
</main>

<footer class="footer">
	<Footer />
</footer>

<style>
	.footer {
		display: flex;
		justify-content: center;
		align-content: center;
	}
</style>
