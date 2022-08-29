<script lang="ts">
	import type { ImageDescriptor } from 'src/types';

	import Image from './Image.svelte';

	export let hash: string;
	export let size = 1;
	export let alt: string;

	export let getImages: () => ImageDescriptor[];
	export let setImages: (images: ImageDescriptor[]) => void;

	function del(event: Event) {
		event.preventDefault();

		if (confirm(`Delete ${alt}?`)) {
			const images = getImages();

			console.log(images.splice(images.map((a) => a.hash).indexOf(hash), 1));

			setImages(images);
		}
	}
</script>

<div>
	<a href="?" on:click={del}>X</a>
	<br />
	<Image {hash} {size} {alt} />
</div>

<style>
	div {
		padding: 3px;
		margin: 4px;

		text-align: right;
	}
</style>
