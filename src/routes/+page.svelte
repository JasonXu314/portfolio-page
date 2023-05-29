<script lang="ts">
	import { onMount } from 'svelte';
	import Typewriter from '../components/typewriter.svelte';
	import { typewriterTimeout } from '../stores';
	import { sleep } from '../utils';

	type Writer = (str: string) => Promise<void>;

	let setHeader: Writer,
		setSub: Writer,
		headFocus = true,
		subFocus = false;

	async function write(writer: Writer, str: string, delay: number): Promise<void> {
		await writer(str);
		return sleep(delay);
	}

	async function playAnimation() {
		await write(setHeader, 'Hi!', 1000);
		await write(setHeader, "I'm Jason Xu", 2000);
		await write(setHeader, 'I', 1000);
		await write(setHeader, 'I.', 1000);
		await write(setHeader, 'I..', 1000);
		await write(setHeader, 'I...', 2000);
		await write(setHeader, 'I make things!', 2000);
		headFocus = false;
		subFocus = true;
		await sleep(2000);
		typewriterTimeout.set(100);
		await write(setSub, 'idk im not very creative lol', 5000);
		headFocus = true;
		subFocus = true;
		typewriterTimeout.set(50);
		await Promise.all([write(setHeader, '', 0), write(setSub, '', 0)]);
		await sleep(500);
		subFocus = false;
		await sleep(2000);
		typewriterTimeout.set(250);

		requestAnimationFrame(playAnimation);
	}

	onMount(playAnimation);
</script>

<main class="container">
	<Typewriter bind:setText={setHeader} element="h1" focused={headFocus} />
	<Typewriter bind:setText={setSub} focused={subFocus} />
</main>
