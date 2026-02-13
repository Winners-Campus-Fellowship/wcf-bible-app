<script lang="ts">
	import { onMount } from 'svelte';

	import Input from './components/home/Input.svelte';
	import Verse from './components/home/VerseNavigator.svelte';

	import type { OSISReference } from '$lib/shared/format';
	import { fetchChapter } from '$lib/bible/chapterServices';
	import { type BibleTranslation, type Verse as VerseType } from '$lib/server/bible';

	let verseLimit = 1;
	let verseReference = '';
	let osis: OSISReference;
	let verseData: VerseType[] = [];
	let selectedVerseIndex: number = 0;
	let selectedTranslation: BibleTranslation = 'NIV';

	async function fetchChapterData(query: string) {
		const updatedData = await fetchChapter({
			input: query,
			verseData,
			verseLimit,
			verseReference,
			selectedVerseIndex,
			selectedTranslation
		})!;

		if (updatedData) {
			osis = updatedData.osis;
			verseData = updatedData.verseData;
			verseLimit = updatedData.verseLimit;
			verseReference = updatedData.verseReference;
			selectedVerseIndex = updatedData.selectedVerseIndex;
		}
	}

	onMount(async () => fetchChapterData('John 1:1')); // this will be the default verse
</script>

<h1>Welcome to SvelteKit</h1>
<p>Visit <a href="https://svelte.dev/docs/kit">svelte.dev/docs/kit</a> to read the documentation</p>

<Input {fetchChapterData} />

<Verse
	{osis}
	{verseData}
	{verseLimit}
	{verseReference}
	{fetchChapterData}
	{selectedVerseIndex}
	{selectedTranslation}
/>
