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

	const colors = {
		text: "#24292e",
		bg: "#ffffff",
		select: "#f2f2f2",
		selectionMatch: "#e0e0e0",
		lineNumber: "#7d7f82",
	};

	onMount(() => {
		const theme = EditorView.theme({
			"&": {
				color: colors.text,
				backgroundColor: colors.bg,
			},
			"&.cm-focused": {
				outline: "none !important",
			},
			".cm-content": {
				padding: "0",
				caretColor: colors.text,
				fontFamily: "Inter Mono, sans-serif",
				lineHeight: "1.5rem",
			},
			"&.cm-focused .cm-cursor": {
				borderLeftColor: colors.text,
			},
			"&.cm-focused .cm-selectionBackground, ::selection, .cm-activeLine, .cm-activeLineGutter":
				{
					backgroundColor: colors.select,
				},
			".cm-gutters": {
				backgroundColor: colors.bg,
				color: colors.lineNumber,
				border: "none",
			},
			".cm-selectionMatch": {
				backgroundColor: colors.selectionMatch,
			},
		});

		editor = new EditorView({
			state: EditorState.create({
				doc: doc,
				extensions: [basicSetup, keymap.of([indentWithTab]), theme],
			}),
			parent: document.getElementById("editor"),
		});

		const editorEl = document.getElementsByClassName("cm-editor")[0];

		editorEl.addEventListener("keyup", () => {
			let text = editor.state.doc.text;

			if (editor.state.doc.children) {
				text = [];

				editor.state.doc.children.forEach((child) => {
					text.push(...child.text);
				});
			}

			doc = text.join("\n");
		});
	});
</script>

<section
	style={`
		width: ${alignVertical ? 100 : editorSize}%;
		height: ${alignVertical ? editorSize : 100}%
	`}
>
	<div id="editor" />
</section>

<style lang="scss">
	section {
		overflow: hidden;

		#editor {
			width: 100%;
			height: 100%;
			overflow: auto;
		}
	}
</style>
