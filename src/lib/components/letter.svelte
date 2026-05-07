<script lang="ts">
	let { letter = $bindable(), activeLetter = $bindable(), index, wordSize } = $props();

  const active = $derived(activeLetter === index);

</script>

{#if active}
	<!-- slot with "underlines" -->
	<div class="rounded-lg border-2 border-black bg-white p-4 text-center">
		<!-- actual letters -->
		<span class="m-2 border-b-2 border-black p-1 text-lg">
			{#if letter != '_'}
				{letter}
			{:else}
				&nbsp;
			{/if}
		</span>
	</div>
{:else}
	<!-- slot with "underlines" -->
	<div class="rounded-lg border border-gray-300 bg-white p-4 text-center" onclick={() => {activeLetter = index;}}>
		<!-- actual letters -->
		<span class="m-2 border-b-2 border-black p-1 text-lg">
			{#if letter != '_'}
				{letter}
			{:else}
				&nbsp;
			{/if}
		</span>
	</div>
{/if}
