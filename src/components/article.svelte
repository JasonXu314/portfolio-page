<script lang="ts">
	import ExternalLink from './external-link.svelte';

	export let links: Record<string, string> = {};
</script>

<article>
	<header>
		<hgroup>
			<slot name="headings" />
		</hgroup>
	</header>
	<slot />
	<footer>
		<slot name="footer">
			{#each Object.entries(links) as [label, href]}
				<ExternalLink {href}>{label}</ExternalLink>
			{/each}
		</slot>
	</footer>
</article>

<style>
	article {
		display: flex;
		flex-direction: column;
		justify-content: space-between;
		margin: calc(var(--block-spacing-vertical) / 4) 0;
	}

	article header hgroup {
		margin-bottom: 0;
	}

	article > :not(header, footer) {
		flex-grow: 1;
	}

	article header {
		margin-bottom: calc(var(--block-spacing-vertical) * 0.5);
		padding: calc(var(--block-spacing-vertical) * 0.5) var(--block-spacing-horizontal);
	}

	article footer {
		margin-top: calc(var(--block-spacing-vertical) * 0.5);
		padding: calc(var(--block-spacing-vertical) * 0.5) var(--block-spacing-horizontal);
		display: flex;
		flex-direction: row;
		gap: var(--block-spacing-horizontal);
	}
</style>
