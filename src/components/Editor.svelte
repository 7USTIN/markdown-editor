<script lang="ts">
	export let alignVertical: boolean;
	export let editorSize: number;
</script>

<section
	style={`
		width: ${alignVertical ? 100 : editorSize}%;
		height: ${alignVertical ? editorSize : 100}%
	`}
>
	<p>Editor</p>
</section>

<style lang="scss">
	section {
		display: flex;
		justify-content: center;
		align-items: center;
		overflow-x: hidden;
	}
</style>
