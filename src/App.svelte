<script lang="ts">
	import { onMount } from "svelte";

	import Editor from "./components/Editor.svelte";
	import Preview from "./components/Preview.svelte";
	import Slider from "./components/Slider.svelte";

	let slider = null;
	let alignVertical = false;
	let editorSize = 50;
	let doc = `### _a minimalistic markdown editor_`;

	const checkIsVertical = () => {
		alignVertical = window.innerHeight > window.innerWidth;
	};
	onMount(checkIsVertical);

	const adjustSize = (ev: any) => {
		let absoluteOffset: number;

		if (ev.type === "mousemove") {
			absoluteOffset = alignVertical
				? ev.clientY - slider.offsetTop
				: ev.clientX - slider.offsetLeft;
		} else {
			const touchEv =
				typeof ev.originalEvent === "undefined" ? ev : ev.originalEvent;
			const touch = touchEv.touches[0] || touchEv.changedTouches[0];

			absoluteOffset = alignVertical
				? touch.pageY - slider.offsetTop
				: touch.pageX - slider.offsetLeft;
		}

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
	on:touchend={() => window.removeEventListener("touchmove", adjustSize)}
	on:mouseup={() => {
		window.removeEventListener("mousemove", adjustSize);
		document.body.style.cursor = "initial";
	}}
/>

<main style={`flex-direction: ${alignVertical ? "column" : "row"}`}>
	<Editor bind:doc {alignVertical} {editorSize} />
	<Preview {alignVertical} {editorSize} {doc} />
	<Slider bind:slider {alignVertical} {editorSize} {adjustSize} />
</main>

<style lang="scss">
	:global(:root) {
		--white: #ffffff;
		--white-dark: #f2f2f2;
		--black: #24292e;
	}

	:global(*) {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
		outline: none;
	}

	:global(body, button, input) {
		font-family: "Inter", sans-serif;
		font-size: 16px;
		background: var(--white);
		color: var(--black);
		overflow: hidden;
	}

	main {
		width: 100vw;
		height: 100vh;
		display: flex;
	}
</style>
