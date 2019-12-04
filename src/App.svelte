<script>
	import { writable } from 'svelte/store';
	import { tweened } from 'svelte/motion';
	import { cubicIn } from 'svelte/easing';
	import { fade, fly, slide, scale } from 'svelte/transition';
	import { flip } from 'svelte/animate';

	import Spring from './Spring.svelte';

	let boxInput;
	let showParagraph = false;


	const progress = tweened(0, {
		delay: 0,
		duration: 700,
		easing: cubicIn
	});

	setTimeout(() => {
		progress.set(0.5);
	}, 1500);

	let boxes = [];

	function addBox() {
		boxes = [boxInput.value, ...boxes]
	}

	function discard(val) {
		boxes = boxes.filter(el => el !== val);
	}
</script>

<style>
	div {
		width: 10rem;
		height: 10rem;
		background: #7B68EE;
		margin: 1rem;
		box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
		border-radius: 5px;
		padding: 1rem;
	}
</style>

<!-- <progress value={$progress} /> -->
<!-- <Spring /> -->

<button on:click={() => showParagraph = !showParagraph}>TOGGLE</button>

{#if showParagraph}
	<p in:fade out:fly={{x: 300}}>Can You See This??</p>
{/if}
<input type="text" bind:this={boxInput}>
<button on:click={addBox}>ADD</button>

{#if showParagraph}
	{#each boxes as box (box)}
		<div
			transition:fly|local={{x: 200, y: 0}}
			on:click={discard.bind(this, box)}
			on:introstart={() => console.log('Adding the element starts')}
			on:introend={() => console.log('Adding the element ends')}
			on:outrostart={() => console.log('Removing the element starts')}
			on:outroend={() => console.log('Removing the elemend ends')}
			animate:flip={{duration: 300}}
		>
			{box}
		</div>
	{/each}
{/if}