<script lang="ts">
	import { marked } from "marked";
	import DOMPurify from "dompurify";

	export let alignVertical: boolean;
	export let editorSize: number;
	export let doc: string;

	$: previewSize = 100 - editorSize;

	marked.setOptions({
		breaks: true,
		gfm: true,
		xhtml: true,
	});
</script>

<section
	style={`
		width: ${alignVertical ? 100 : previewSize}%;
		height: ${alignVertical ? previewSize : 100}%
	`}
>
	<div class="preview">
		{@html DOMPurify.sanitize(marked.parse(doc))}
	</div>
</section>

<style lang="scss">
	section {
		overflow: hidden;

		.preview {
			width: 100%;
			height: 100%;
			overflow: auto;
			white-space: nowrap;
			padding: 32px;
			line-height: 1.5rem;
		}
	}
</style>
