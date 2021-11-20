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

	const adjustSize = (ev: any) => {
		let absoluteOffset: number;

		if (ev.type === "mousemove") {
			absoluteOffset = alignVertical
				? ev.clientY - slider.offsetTop
				: ev.clientX - slider.offsetLeft;
		} else {
			const tEv =
				typeof ev.originalEvent === "undefined" ? ev : ev.originalEvent;
			const touch = tEv.touches[0] || tEv.changedTouches[0];

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
	<Editor {alignVertical} {editorSize} />
	<Preview {alignVertical} {editorSize} />

	<div
		on:mousedown={() => window.addEventListener("mousemove", adjustSize)}
		on:touchstart={() => window.addEventListener("touchmove", adjustSize)}
		bind:this={slider}
		class="slider"
		style={`
			top: ${alignVertical ? editorSize + "%" : "0"};
			left: ${alignVertical ? "0" : editorSize + "%"};
			width: ${alignVertical ? "100%" : "2px"};
			height: ${alignVertical ? "2px" : "100%"};
			transform: translate${alignVertical ? "Y" : "X"}(-50%);
			cursor: ${alignVertical ? "row-resize" : "col-resize"}
		`}
	>
		<div
			class="transparent"
			style={`transform: translate${alignVertical ? "Y" : "X"}(-50%);`}
		/>

		<div
			class="drag-handle"
			style={alignVertical
				? `transform-origin: 85% 15%; transform: rotate(90deg);`
				: ""}
		>
			<i class="material-icons">drag_indicator</i>
		</div>
	</div>
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
			user-select: none;
			-moz-user-select: none;
			-webkit-user-select: none;

			.transparent {
				width: 100%;
				height: 100%;
				border: 16px solid transparent;
			}

			.drag-handle {
				position: absolute;
				top: 50%;
				left: 50%;
				transform: translate(-50%, -50%);
				background: var(--black);
				border-radius: 3px;
				overflow: hidden;
				width: 10px;
				height: 28px;
				display: flex;
				align-items: center;
				justify-content: center;

				i {
					color: var(--white);
					font-size: 14px;
				}
			}
		}
	}
</style>
