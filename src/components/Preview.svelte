<script lang="ts">
	export let alignVertical: boolean;
</script>

<section
	style={`
		width: ${alignVertical ? 100 : 50}%;
		height: ${alignVertical ? 50 : 100}%
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
	}
</style>
