<script lang="ts">
	import { onMount } from "svelte";

	import Editor from "./components/Editor.svelte";
	import Preview from "./components/Preview.svelte";

	let alignVertical = false;

	const checkWinDimensions = () => {
		alignVertical = window.innerHeight > window.innerWidth;
	};
	onMount(checkWinDimensions);
</script>

<svelte:window on:resize={checkWinDimensions} />

<main style={`flex-direction: ${alignVertical ? "column" : "row"}`}>
	<Editor {alignVertical} />
	<Preview {alignVertical} />
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
		font-family: "Noto Sans Mono", sans-serif;
		font-size: 14px;
		background: var(--white);
		color: var(--black);
	}

	main {
		display: flex;
		width: 100vw;
		height: 100vh;
	}
</style>
