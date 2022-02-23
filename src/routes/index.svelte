<script lang="ts">
	import { onDestroy, onMount } from 'svelte';
	import Footer from '$lib/Footer.svelte';
	let showToolbar = true;
	let textele;
	let editor: EasyMDE;

	// Setting up the editor and the visible buttons of the toolbar
	onMount(async () => {
		const easymde = await import('easymde');
		const EasyMDE = easymde.default;
		editor = new EasyMDE({
			element: textele,
			autofocus: true,
			spellChecker: false,
			nativeSpellcheck: true,
			theme: 'easymde',
			autosave: {
				enabled: true,
				uniqueId: 'easymde-autosave',
				delay: 1000
			},
			toolbar: [
				'preview',
				'|',
				'bold',
				'italic',
				'strikethrough',
				'|',
				'heading-bigger',
				'heading-smaller',
				'heading-1',
				'heading-2',
				'heading-3',
				'|',
				'unordered-list',
				'ordered-list',
				'|',
				'code',
				'image',
				'link',
				'quote',
				'table',
				'horizontal-rule',
				'|',
				'redo',
				'undo',
				'guide',
				'|',
				// Adding custom buttons
				{
					name: 'clipboard',
					action: saveToClipboard,
					className: 'fa fa-clipboard',
					title: 'Copy entire text to clipboard'
				},
				{
					name: 'clear',
					action: clearEditor,
					className: 'fa fa-trash',
					title: 'Clear the editor'
				}
			],
			status: ['autosave', 'words'],
			hideIcons: ['fullscreen', 'side-by-side'],
			placeholder:
				'Hello! \n\n This is your distraction free markdown editor.\n You can use it to write your blog posts or to write your own documentation.\n It supports standard markdown syntax.\n Toggle the button in the top left corner to enter Zen Mode. All UI elements will be hidden, so you can focus on what is important - Your text.'
		});
	});
	// Resetting the editor when clearing it
	onDestroy(() => {
		if (editor) {
			editor.toTextArea();
			editor = null;
		}
	});

	// Fallback solution if the modern method of copying to clipboard fails
	function fallbackCopyTextToClipboard(text) {
		var textArea = document.createElement('textarea');
		textArea.value = text;

		// Avoid scrolling to bottom
		textArea.style.top = '0';
		textArea.style.left = '0';
		textArea.style.position = 'fixed';

		document.body.appendChild(textArea);
		textArea.focus();
		textArea.select();

		// Errorhandling for copy to clipboard function support on different browsers
		try {
			var successful = document.execCommand('copy');
			var msg = successful ? 'successful' : 'unsuccessful';
			console.log('Fallback: Copying text command was ' + msg);
		} catch (err) {
			console.error('Fallback: Oops, unable to copy', err);
		}

		document.body.removeChild(textArea);
	}
	// Copy content to clipboard
	function copyTextToClipboard(text) {
		if (!navigator.clipboard) {
			fallbackCopyTextToClipboard(text);
			return;
		}
		navigator.clipboard.writeText(text).then(
			function () {
				console.log('Async: Copying to clipboard was successful!');
			},
			function (err) {
				console.error('Async: Could not copy text: ', err);
			}
		);
	}

	function saveToClipboard() {
		const content = editor.value();
		copyTextToClipboard(content);
	}

	// Clearing the editor textarea
	function clearEditor() {
		editor.value('');
	}
</script>

<svelte:head>
	<link rel="stylesheet" href="/easymde.css" />
	<title>Zen Markdown Editor</title>
</svelte:head>

<!-- This creates a sticky button on the page that toggles Zen Mode -->
<div class="sticky-button">
	<button class="hide-toggle" on:click={() => (showToolbar = !showToolbar)}>
		<img src="/zenlogo.svg" alt="Toggle Zen Mode" />
		<span>Zen Mode</span>
	</button>
</div>

<img src="/writer.jpg" alt="Zen Mode Logo" class="zen-logo" />

<!-- Everything in here will hide, when the Zen Mode is toggled on. -->
<section class:hidden-tool={!showToolbar}>
	<img class="clicktotoggle" src="/clicktotoggle.png" alt="Click to toggle tooltip" />
	<div class="editor"><textarea id="invisible text area" bind:this={textele} /></div>
	<img class="logo" src="/header.svg" alt="Zen MarkDown Editor" />
	<footer class="footer">
		<Footer />
	</footer>
</section>

<style>
	:global(.hidden-tool .editor .editor-toolbar) {
		display: none;
	}
	.logo {
		width: calc(max(min(92vw, 780px), 400px));
		margin-left: calc(max(min(92vw, 780px), 400px) / -2);
		height: calc(max(min(92vw, 780px), 400px) / 4);
		display: block;
		top: 50px;
		left: 50%;
		opacity: 1;
		transition: opacity 1s ease-in-out;
		z-index: -10;
		position: absolute;
	}

	.hidden-tool .editor {
		margin-top: 0px;
	}

	.editor {
		margin-top: calc(max(min(92vw, 780px), 400px) / 4);
		transition: margin-top 1s ease-in-out;
		margin-bottom: 70px;
	}

	.hidden-tool .logo {
		opacity: 0;
	}

	.clicktotoggle {
		top: 6em;
		left: 2em;
		position: fixed;
		width: 180px;
		height: 180px;
		opacity: 1;
		transition: opacity 1s ease-in-out;
		z-index: -40;
	}

	.hidden-tool .clicktotoggle {
		opacity: 0;
	}

	.sticky-button {
		top: 3em;
		position: sticky;
		z-index: 20;
		display: inline-block;
	}

	.hide-toggle {
		position: relative;
		height: 50px;
		width: 50px;
		margin: 10px 0;
		padding: 7px 0 0 8px;
		border: 1px solid #1f1c22;
		border-left: 0;
		background: #190b28;
		overflow: hidden;
		transition: width 1s ease-in-out;
		box-sizing: padding-box;
		box-sizing: border-box;
		border-radius: 0px 25px 25px 0px;
	}

	.hide-toggle img {
		width: 29px;
		height: 29px;
		position: absolute;
		right: 9px;
		top: 10px;
		background-color: #190b28;
		z-index: 30;
		padding-right: 10px;
		padding-left: 5px;
		margin-right: -10px;
	}

	.hide-toggle:hover {
		width: 170px;
	}

	.hide-toggle span {
		opacity: 0;
		position: absolute;
		top: 14px;
		left: 10px;
		transition: opacity 1s linear 0s;
		font-family: 'Libre Baskerville', serif;
		font-size: 1.4em;
		white-space: nowrap;
	}

	.hide-toggle:hover span {
		opacity: 1;
	}

	.footer {
		display: flex;
		justify-content: center;
		align-content: center;
		opacity: 1;
		transition: opacity 1s ease-in-out;
	}

	.hidden-tool .footer {
		opacity: 0;
	}
</style>
