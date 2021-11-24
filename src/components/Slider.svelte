<script lang="ts">
	export let alignVertical: boolean;
	export let slider: HTMLElement;
	export let adjustSize: Function;
	export let editorSize: number;
</script>

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

<style lang="scss">
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

			i {
				color: var(--white);
				font-size: 14px;
			}
		}
	}
</style>
