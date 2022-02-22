<script lang="ts">
	import { SvelteComponent } from 'svelte';
	// import showToolbar from 'src/routesindex.svelte';
</script>

<div class="footer">
	<!-- create a collapsable text block -->
	<div class="collapse" id="collapseExample">
		<div class="card card-body">
			<p>
				This website was created as part of the <a href="https://hashnode.com">hashnode</a> and
				<a href="http://netlify.com">netlify</a> hackathon. The blogpost on this site was created is
				available at <a href="http://blog.tiia.rocks">my blog</a>.
				<br />
				Author: <a href="https://twitter.com/tiiaaurora" target="_blank">@tiiaaurora</a>
				GitHub: <a href="https://github.com/tiiaaurora" target="_blank">TiiaAurora</a>
			</p>
		</div>
	</div>
</div>

<style>
	.footer {
		position: fixed;
		bottom: 0;
		width: 100%;
		height: auto;
		background-color: #201c29;
		color: #e1bffd;
		text-align: right;
		max-width: 1000px;
		min-width: 400px;
		padding: 10px;
		bottom: 0;

		box-shadow: black 0px 0px 10px;
	}

	.footer p {
		margin-bottom: 0;
		font-family: 'Roboto', sans-serif;
		font-size: small;
		font-style: normal;
		padding-right: 20px;
	}

	.footer a {
		color: white;
		text-decoration: none;
	}
</style>
