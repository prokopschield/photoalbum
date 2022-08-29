<script lang="ts">
	export let hash: string;
	export let size = 1;
	export let alt: string;

	$: width = Math.floor((window.innerWidth * size) / 7);
	$: height = Math.floor((width * 2) / 3);
	$: link = `https://webshot.nodesite.eu:20122/rest/${width}/${height}/full/https://cdn.discordapp.sk/static/${hash}`;
</script>

<a target="_blank" href="https://cdn.discordapp.sk/static/{hash}">
	<img src={link} {alt} />
</a>

<style>
</style>
