<script lang="ts">
	import { Remarkable } from "remarkable";
	const md = new Remarkable({
		breaks: false,
		html: true,
	});

	export let alignVertical: boolean;
	export let editorSize: number;
	export let doc: string;

	$: previewSize = 100 - editorSize;
</script>

<section
	style={`
		width: ${alignVertical ? 100 : previewSize}%;
		height: ${alignVertical ? previewSize : 100}%
	`}
>
	<div class="preview">
		{@html md.render(doc)}
	</div>
</section>

<style lang="scss">
	section {
		overflow: hidden;

		.preview {
			width: 100%;
			height: 100%;
			overflow: auto;
			padding: 32px;
			line-height: 1.5rem;
		}
	}
</style>
