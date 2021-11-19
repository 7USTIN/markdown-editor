<script lang="ts">
</script>

<main>
	<h1>Hello World</h1>
</main>

<style>
	:global(:root) {
		--white: #ffffff;
		--white-dark: #f2f2f2;
		--black: #24292e;
		--gray: #babbbd;
	}

	:global(*) {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
		outline: none;
	}

	:global(body, button, input) {
		font-family: "Inter", sans-serif;
		font-size: 14px;
		background: var(--white);
		color: var(--black);
	}
</style>
