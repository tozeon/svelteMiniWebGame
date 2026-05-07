<script lang="ts">
	import Letter from '$lib/components/letter.svelte';

	const word = 'tiger';
	const userInput: Array<{ index: number; value: string }> = $state([]);
	let activeLetter = $state(0);
	const updateLetter = (index: number, value: string) => {
		userInput[index].value = value;
	};

	word.split('').forEach((_, index) => {
		userInput.push({ index, value: '_' });
	});
	userInput[0].value = 't';
</script>

<div class="flex h-screen w-screen items-center justify-center bg-blue-200">
	<div class="flex h-fit w-fit items-center justify-center">
		<div class="flex flex-row gap-2">
			{#each userInput as letter (letter.index)}
				<Letter bind:letter={letter.value} bind:activeLetter={activeLetter} index={letter.index} wordSize={word.length} />
			{/each}
		</div>
	</div>
</div>
