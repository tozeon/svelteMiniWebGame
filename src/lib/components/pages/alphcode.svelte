<script lang="ts">
	import Letter from '$lib/components/letter.svelte';
	import { ALPHABET } from '$lib/util/alphabet';

	const word = 'tiger';

	// user input and the letter they are on
	const userInput: Array<{ index: number; value: string }> = $state([]);
	let activeLetter = $state(0);

	// temp list
	const codes: Array<{ index: number; value: string }> = [];

	ALPHABET.split('').forEach((letter, index) => {
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

	const onKeydown = (e: KeyboardEvent) => {
		if (e.target instanceof HTMLInputElement || e.target instanceof HTMLTextAreaElement) return;

		const current = activeLetter;

		if (ALPHABET.includes(e.key)) {
			userInput[current].value = e.key;
			activeLetter = Math.min(current + 1, word.length - 1);
		} else {
			switch (e.key) {
				case 'Backspace':
					if (userInput[current].value !== '_') {
						userInput[current].value = '_';
						activeLetter = Math.max(current - 1, 0);
					} else {
						activeLetter = Math.max(current - 1, 0);
					}

					break;
				case 'Enter':
					activeLetter = Math.min(current + 1, word.length - 1);
					break;
				case 'Tab':
					e.preventDefault();
					break;
			}
		}
	};
</script>

<div
	class="flex h-screen w-screen items-center justify-center bg-blue-200"
	role="menu"
	onkeydown={onKeydown}
	tabindex="-1"
	onclick={() => {
		console.log('clicked');
	}}
>
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
						<Letter letter={letter.value} {activeLetter} index={letter.index} />
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
