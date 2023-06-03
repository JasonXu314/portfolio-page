<script lang="ts">
	export let target: string;
</script>

<a href={target}>
	<svg
		xmlns="http://www.w3.org/2000/svg"
		width={96}
		height={96}
		viewBox="0 0 24 24"
		fill="none"
		stroke="white"
		stroke-width="2"
		stroke-linecap="round"
		stroke-linejoin="round"
		class="feather feather-chevron-down"
	>
		<polyline points="6 9 12 15 18 9" />
	</svg>
</a>

<style>
	@keyframes bounce {
		0% {
			translate: 0 0;
		}

		100% {
			translate: 0 50px;
		}
	}

	a {
		display: block;
		position: absolute;
		transform: translateX(-50%);
		left: 50%;
		bottom: 5%;

		animation: bounce 0.75s cubic-bezier(0, 0, 0.5, 0) infinite alternate-reverse;
	}
</style>
