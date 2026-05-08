<script lang="ts">
	let { letter = $bindable(), activeLetter = $bindable(), index } = $props();

	const active = $derived(activeLetter === index);

</script>

<div
	class="cursor-pointer rounded-lg border-2 border-black bg-white p-4 text-center"
	class:border-black={active}
	class:border-gray-300={!active}
>
	<span class="m-2 border-b-2 border-black p-1 text-lg">
		{#if letter != '_'}
			{letter}
		{:else}
			&nbsp;
		{/if}
	</span>
</div>
