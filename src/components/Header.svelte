<script lang="ts">
	import type { ImageDescriptor } from 'src/types';

	export let size: number;
	export let getImages: () => ImageDescriptor[];
	export let setImages: (images: ImageDescriptor[]) => void;

	let files: FileList;

	async function upload() {
		for (let i = 0; i < files.length; ++i) {
			const file = files.item(i);

			const response = await fetch('https://cdn.discordapp.sk/api/upload', {
				method: 'PUT',
				headers: {
					'Content-Type': file.type,
				},
				body: file,
			});

			const hash = await response.text();

			setImages([{ alt: file.name, hash }, ...getImages()]);
		}
	}

	$: if (files && files[0]) {
		upload();
	}

	let copyclass = '';

	function copy() {
		navigator.clipboard
			.writeText(location.href)
			.then(() => {
				copyclass = 'copied';

				setTimeout(() => {
					copyclass = '';
				}, 1200);
			})
			.catch((err) => {
				console.error('Error in copying text: ', err);
			});
	}
</script>

<header>
	<button on:click={() => size > 1 && --size}>-</button>
	<button on:click={() => size < 7 && ++size}>+</button>

	<button on:click={upload}>Upload</button>
	<input type="file" bind:files multiple />

	<button class={copyclass} on:click={copy}>Copy Link</button>
</header>

<style>
	header {
		margin-bottom: 2.3em;
	}
	.copied {
		background-color: lightgreen;
	}
</style>
