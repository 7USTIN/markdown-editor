<script lang="ts">
	import { marked } from "marked";
	import DOMPurify from "dompurify";

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
		{@html DOMPurify.sanitize(marked.parse(doc))}
	</div>
</section>

<style lang="scss">
	section {
		overflow: hidden;

		.preview {
			white-space: nowrap;
			margin: 32px;
		}
	}
</style>
