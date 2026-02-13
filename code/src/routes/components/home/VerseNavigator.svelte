<script lang="ts">
	import { getVerseReference } from '$lib/bible/chapterServices';
	import YVTranslations from '$lib/shared/YVTranslations.json' with { type: 'json' };
	import APIBibleTranslations from '$lib/shared/APIBibleTranslations.json' with { type: 'json' };

	let {
		osis,
		verseData,
		verseLimit,
		verseReference,
		fetchChapterData,
		selectedVerseIndex,
		selectedTranslation
	} = $props();

	const translations = [
		...new Set([...Object.keys(YVTranslations), ...Object.keys(APIBibleTranslations)])
	].sort();
</script>

<div>
	<p>{verseReference}</p>
	<p>{verseData[selectedVerseIndex]?.text}</p>
</div>

<button
	id="button"
	onclick={() => {
		selectedVerseIndex -= 1;
		verseReference = getVerseReference(verseData, osis, selectedVerseIndex);
	}}
	disabled={selectedVerseIndex == 0}>Previous</button
>
<button
	onclick={() => {
		selectedVerseIndex += 1;
		verseReference = getVerseReference(verseData, osis, selectedVerseIndex);
	}}
	disabled={selectedVerseIndex == verseLimit - 1}>Next</button
>

<select
	bind:value={selectedTranslation}
	onchange={() => {
		fetchChapterData(verseReference);

		// verse reference is updated again to handle the case where the user changes to a translation that
		// doesn't have the selected verse
		verseReference = getVerseReference(verseData, osis, selectedVerseIndex);
	}}
>
	{#each translations as translation (translation)}
		<option value={translation}>{translation}</option>
	{/each}
</select>
