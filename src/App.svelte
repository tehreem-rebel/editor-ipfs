<script>
	export let name;
	import 'suneditor/dist/css/suneditor.min.css'
	import suneditor from 'suneditor'
	import plugins from 'suneditor/src/plugins'
	import IPFS from 'ipfs-mini';
	import list from 'suneditor/src/plugins/submenu/list'
	import lang from 'suneditor/src/lang'
	import { onMount } from 'svelte';

	let IPFS_DATA = "";

	async function onUploadDataOnIPFS() {
		try {
			const ipfs = new IPFS({ host: 'ipfs.infura.io', port: 5001, protocol: 'https' });
			ipfs.add(IPFS_DATA ,(err, result) => {
				console.log(`https://ipfs.infura.io/ipfs/${result}`);
				console.log('error: ', err);
			});
		} catch (error) {
			console.log('Error uploading file: ', error)
		}
	}

	onMount(() => {
		 let editor = suneditor.create("sample", {
			 plugins: plugins,
			 buttonList: [
				 [
					 'undo', 'redo',
					 'font', 'fontSize', 'formatBlock',
					 'paragraphStyle', 'blockquote',
					 'bold', 'underline', 'italic',
					 'fontColor', 'hiliteColor', 'textStyle',
					 'outdent', 'indent',
					 'align', 'list', 'lineHeight',
					 'link', 'image',
					 'save',
				 ]
			 ],
			 lang: lang.en,
			height: 700,
			width: 1500,
		})

		editor.onSave = (contents ) => {
			IPFS_DATA = contents;
		 }

	})

</script>

<main>
	<h1>Svelte {name} Of Editor</h1>
	<textarea id="sample" ></textarea>

	<h1>IPFS</h1>
<!--	<input type="file">-->
	<h3> To Upload Data on IPFS: </h3>
	<button on:click={onUploadDataOnIPFS}>upload</button>

	<!--{@html IPFS_DATA}-->

</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>