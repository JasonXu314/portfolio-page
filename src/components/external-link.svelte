<script lang="ts">
	export let href: string;
</script>

<a {href} target="_blank" rel="noreferrer noopener" class="external">
	<slot>{href}</slot>
</a>

<style>
	.external::after {
		content: '';
		display: inline-block;
		height: 16px;
		width: 16px;
		background-image: url('/external-link.svg');
		background-size: contain;
		margin-left: 0.5ch;
	}
</style>
