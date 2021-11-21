<script lang="ts">
	import {
		EditorState,
		EditorView,
		basicSetup,
	} from "@codemirror/basic-setup";
	import { keymap } from "@codemirror/view";
	import { indentWithTab } from "@codemirror/commands";
	import { onMount } from "svelte";

	export let alignVertical: boolean;
	export let editorSize: number;
	export let doc: string;

	let editor = null;

	onMount(() => {
		editor = new EditorView({
			state: EditorState.create({
				doc: doc,
				extensions: [basicSetup, keymap.of([indentWithTab])],
			}),
			parent: document.getElementById("editor"),
		});

		const editorEl = document.getElementsByClassName("cm-editor")[0];

		editorEl.addEventListener("keyup", () => {
			doc = editor.state.doc.text.join("\n\n");
		});
	});
</script>

<section
	id="editor"
	style={`
		width: ${alignVertical ? 100 : editorSize}%;
		height: ${alignVertical ? editorSize : 100}%
	`}
/>

<style lang="scss">
	section {
	}
</style>
