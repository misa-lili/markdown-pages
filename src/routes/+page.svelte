<script>
	import { marked } from 'marked';
	import { onMount } from 'svelte';

	let input = '';
	let markdown = '';
	let html = '';

	let script = '';
	let style = '';

	let isMounted = false;

	$: if (isMounted && input) {
		markdown = input;
		runScript();
		updateStyle();
		html = marked.parse(markdown);
	}

	onMount(async () => {
		input = await loadMarkdown();
		isMounted = true;
	});

	async function loadMarkdown() {
		const res = await fetch('/test.md');
		return await res.text();
	}

	function updateStyle() {
		try {
			style = input.match(/<style>((?:(?!<style>).)*?)<\/style>/s)[1];
			const styleTag = document.createElement('style');
			styleTag.innerHTML = style;
			document.head.appendChild(styleTag);
		} catch (e) {
			console.error(e);
		}
	}

	function runScript() {
		try {
			script = input.match(/<script>([\s\S]*)<\/script>/m)[1];
			eval(script);
		} catch (e) {
			console.error('An error occurred:', e);
		}
	}

	function handleKeydown(event) {
		if (event.key === 'Tab') {
			event.preventDefault();
			document.execCommand('insertText', false, '\t');
		}
	}
</script>

<div class="flex">
	<div
		class="container"
		id="editor"
		contenteditable="true"
		bind:innerText={input}
		on:keydown={handleKeydown}
	/>
	<div class="container" id="preview--container">
		{@html html}
	</div>
</div>

<div id="tool-bar">
	<button>tab</button>
	<button>vertical bar</button>
</div>

<style>
	:global(html, body) {
		margin: 0;
		padding: 0;
	}

	#editor {
		white-space: pre-wrap;
		/* tab-size: 2; */
		font-family: 'Courier New', Courier, monospace;
		outline-width: 0;
	}

	#tool-bar {
		position: fixed;
		top: 0px;
		line-height: 40px;
		text-align: center;
		left: 50%;
		transform: translateX(-50%);
	}

	#run-script--button {
		width: 50px;
		height: 50px;
		background: #ccc;
		border-radius: 50%;
		display: flex;
		justify-content: center;
		align-items: center;
		cursor: pointer;
	}

	.flex {
		display: flex;
		overflow: hidden;
	}

	.flex .container {
		flex: 1;
		border: 1px solid #ccc;
		padding: 10px;
		width: 50%;
		height: 100vh;
		overflow: scroll;
	}

	*:focus {
		outline: 0;
	}
</style>
