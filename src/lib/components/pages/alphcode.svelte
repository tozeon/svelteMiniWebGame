<script lang="ts">
	import Letter from '$lib/components/letter.svelte';

	const word = 'tiger';

	// user input and the letter they are on
	const userInput: Array<{ index: number; value: string }> = $state([]);
	let activeLetter = $state(0);

	// temp list
	const codes: Array<{ index: number; value: string }> = [];
	const alphabet = 'abcdefghijklmnopqrstuvwxyz';

	alphabet.split('').forEach((letter, index) => {
		codes.push({ index: index + 1, value: letter });
	});

	word.split('').forEach((_, index) => {
		userInput.push({ index, value: '_' });
	});

	// encrypt into numbers
	let encryptedWord: number[] = $state([]);
	word.split('').forEach((letter) => {
		const code = codes.find((c) => c.value === letter);
		if (code) {
			encryptedWord.push(code.index);
		}
		console.log(code);
	});
</script>

<div class="flex h-screen w-screen items-center justify-center bg-blue-200">
	<aside class="mr-16 ml-8 flex flex-1 flex-col border">
		<div class="w-full flex-col p-16">
			<h1>Codes</h1>
			<ul class="grid grid-cols-5 gap-2">
				{#each codes as code (code.index)}
					<li><span>{code.index}: {code.value}</span></li>
				{/each}
			</ul>
		</div>
	</aside>
	<main class="flex h-full flex-col items-center justify-center gap-2">
		<div class="flex h-fit w-fit items-center justify-center">
			<div class="flex flex-row gap-2">
				{#each userInput as letter (letter.index)}
					<div class="flex flex-col items-center justify-center gap-3">
						<!-- show the encrypted word -->
						<span>{encryptedWord[letter.index]}</span>
						<Letter
							bind:letter={letter.value}
							bind:activeLetter
							index={letter.index}
							wordSize={word.length}
						/>
					</div>
				{/each}
			</div>
		</div>
		<button
			class="cursor-pointer rounded bg-blue-500 px-4 py-2 font-bold text-white hover:bg-blue-700"
		>
			submit
		</button>
	</main>
	<aside class="flex-1">
		<p class="text-sm"></p>
	</aside>
</div>
