<script lang="ts">
	import Header from './components/Header.svelte';
	import ImageContainer from './components/ImageContainer.svelte';

	import type { ImageDescriptor } from './types';

	let images = new Array<ImageDescriptor>();

	let [hash] = location.href.match(/[a-f0-9]{64}/g) || [''];

	function updateHash(images: ImageDescriptor[]) {
		if (images.length) {
			fetch('https://cdn.nodesite.eu:20122/put', {
				method: 'POST',
				body: JSON.stringify(images),
			})
				.then((res) => res.json())
				.then((res) => res.hash !== hash && (hash = res.hash));
		}
	}

	$: updateHash(images);

	function updateImages(hash: string) {
		if (hash) {
			fetch(`https://cdn.nodesite.eu:20122/${hash}`, {
				method: 'GET',
			})
				.then((res) => res.json())
				.then((res) => (images = res));
		}
	}

	$: updateImages(hash);

	$: history.pushState('', '', '/?' + hash);

	function getImages() {
		return images;
	}

	function setImages(new_images: ImageDescriptor[]) {
		images = new_images;
	}

	let size = 4;
</script>

<main>
	<Header bind:size {getImages} {setImages} />

	<div class="flex_container">
		{#each images as { alt, hash }}
			<ImageContainer {getImages} {setImages} {alt} {hash} {size} />
		{/each}
	</div>
</main>

<style>
	.flex_container {
		display: flex;
		flex-wrap: wrap;
	}
</style>
