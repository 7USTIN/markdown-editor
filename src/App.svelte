<script lang="ts">
	import { onMount } from "svelte";

	import Editor from "./components/Editor.svelte";
	import Preview from "./components/Preview.svelte";

	let slider: HTMLElement;
	let alignVertical = false;
	let editorSize = 50;

	const checkIsVertical = () => {
		alignVertical = window.innerHeight > window.innerWidth;
	};
	onMount(checkIsVertical);

	const adjustSize = (ev: MouseEvent) => {
		const absoluteOffset = alignVertical
			? ev.clientY - slider.offsetTop
			: ev.clientX - slider.offsetLeft;
		const windowSize = alignVertical
			? window.innerHeight
			: window.innerWidth;
		const relativeOffset = (absoluteOffset / windowSize) * 100;

		if (
			(editorSize + relativeOffset <= 98 && relativeOffset > 0) ||
			(editorSize + relativeOffset >= 2 && relativeOffset < 0)
		) {
			editorSize += relativeOffset;
		}
	};

	const changeCursor = (ev: MouseEvent) => {
		if (ev.target === slider) {
			document.body.style.cursor = alignVertical
				? "row-resize"
				: "col-resize";
		}
	};
</script>

<svelte:window
	on:resize={checkIsVertical}
	on:mousedown={changeCursor}
	on:mouseup={() => {
		window.removeEventListener("mousemove", adjustSize);
		document.body.style.cursor = "initial";
	}}
/>

<main style={`flex-direction: ${alignVertical ? "column" : "row"}`}>
	<Editor {alignVertical} {editorSize} />
	<Preview {alignVertical} {editorSize} />

	<div
		on:mousedown={() => window.addEventListener("mousemove", adjustSize)}
		bind:this={slider}
		class="slider"
		style={`
			top: ${alignVertical ? editorSize + "%" : "0"};
			left: ${alignVertical ? "0" : editorSize + "%"};
			width: ${alignVertical ? "100%" : "3px"};
			height: ${alignVertical ? "3px" : "100%"};
			cursor: ${alignVertical ? "row-resize" : "col-resize"}
		`}
	/>
</main>

<style lang="scss">
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
		overflow: hidden;
	}

	main {
		width: 100vw;
		height: 100vh;
		display: flex;

		.slider {
			background: var(--black);
			position: absolute;
		}
	}
</style>
