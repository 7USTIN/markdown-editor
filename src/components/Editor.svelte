<script lang="ts">
	export let alignVertical: boolean;
</script>

<section
	style={`
		width: ${alignVertical ? 100 : 50}%;
		height: ${alignVertical ? 50 : 100}%
	`}
>
	<p>Editor</p>
</section>

<style lang="scss">
	section {
		background: lightcyan;
		display: flex;
		justify-content: center;
		align-items: center;
	}
</style>
