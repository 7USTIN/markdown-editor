<script lang="ts">
	export let alignVertical: boolean;
	export let editorSize: number;

	$: previewSize = 100 - editorSize;
</script>

<section
	style={`
		width: ${alignVertical ? 100 : previewSize}%;
		height: ${alignVertical ? previewSize : 100}%
	`}
>
	<p>Preview</p>
</section>

<style lang="scss">
	section {
		background: lavender;
		display: flex;
		align-items: center;
		justify-content: center;
		overflow: hidden;
	}
</style>
