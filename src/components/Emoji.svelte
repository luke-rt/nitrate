<script lang="ts">
	import { onMount } from "svelte";
	import { invoke } from "@tauri-apps/api/tauri"
	import { appWindow } from "@tauri-apps/api/window";

	export let img: string;

	let img_data;

	const copy = () => {
		/**
		 * Copies the image to the clipboard
		*/
		invoke("copy_image", {
			filename: img,
		});
		appWindow.minimize();
	};

	onMount(async () => {
		/**
		 * Runs once on the creation of an Emoji component
		 * Gets the image data in Base64 String
		 * @returns {Promise<void>}
		*/
		img_data = await invoke("get_image_data", {
			filename: img,
		});
	});
</script>

<button on:click={copy}>
	<img src={img_data} alt="img"/>
</button>

<style lang="scss">
	button {
		cursor: pointer;

		border: none;
		padding: 4px;
		border-radius: 1.5vw;
		border: none;

		transition: background-color 0.2s;

		&:hover {
			background-color: $text-color-hover;
			img {
				background-color: $text-color-hover;
			}
		}
	}

	img {
		transition: background-color 0.2s;

		height: 50px;
		width: 50px;
	}
</style>
