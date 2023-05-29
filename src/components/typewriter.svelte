<script lang="ts">
	import { typewriterTimeout } from '../stores';

	export let element = 'p',
		focused: boolean;

	let text = '',
		unTypingIdx = -1,
		toText = '',
		typing = false,
		resolver: (() => void) | null = null,
		promise: Promise<void> | null = null;

	export const setText = (str: string): Promise<void> => {
		toText = str;

		unTypingIdx = -1;
		for (let i = 0; i < Math.min(toText.length, text.length); i++) {
			if (text.slice(0, i) !== toText.slice(0, i)) {
				unTypingIdx = i;
				break;
			}
		}

		if (unTypingIdx === -1 && toText.length < text.length) {
			unTypingIdx = toText.length + 1;
		}

		if (!typing) {
			typing = true;
			type();
			const p = new Promise<void>((resolve) => {
				resolver = resolve;
			});
			promise = p;
			return p;
		} else {
			return promise!;
		}
	};

	function type() {
		if (unTypingIdx !== -1 && text.length >= unTypingIdx) {
			text = text.slice(0, -1);
			setTimeout(type, $typewriterTimeout);
		} else if (text.length < toText.length) {
			if (unTypingIdx !== -1) {
				unTypingIdx = -1;
			}

			text = text + toText.charAt(text.length);
			setTimeout(type, $typewriterTimeout);
		} else {
			typing = false;
			if (resolver) {
				resolver();
			}
		}
	}
</script>

<svelte:element this={element}>
	{text}
	{#if focused}
		<span class="cursor" />
	{/if}
</svelte:element>

<style>
	@keyframes blink {
		0% {
			border-left-color: rgba(255, 255, 255, 1);
		}

		50% {
			border-left-color: rgba(255, 255, 255, 0);
		}

		100% {
			border-left-color: rgba(255, 255, 255, 1);
		}
	}

	.cursor {
		border-left: 1px solid;
		animation: 1.5s blink forwards infinite;
	}
</style>
