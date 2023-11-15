<script>
	// import { onMount } from 'svelte';

	let tracks = [];
	let artist = '';
	let track = '';
	let isLoading = false;
	let isWrong = false;
	let isNotFound = false;

	console.log(
		`https://ws.audioscrobbler.com/2.0/?method=track.getsimilar&artist=${artist}&track=${track}&api_key=ae4f9bac310f0d968cd4f48f80abd386&format=json`
	);

	console.log(
		'https://ws.audioscrobbler.com/2.0/?method=artist.getcorrection&artist=Shkira&api_key=ae4f9bac310f0d968cd4f48f80abd386&format=json'
	);

	function searchTracks() {
		//zodat ik geen error krijg als ik perongeluk een spatie na de naam van de artiest of track zet.
		artist = artist.trim();
		track = track.trim();

		if (artist && track) {
			isLoading = true;

			// artist correction api last.fm
			fetch(`https://ws.audioscrobbler.com/2.0/?method=artist.getcorrection&artist=${artist}&api_key=ae4f9bac310f0d968cd4f48f80abd386&format=json`)
			.then((res) => res.json())
			.then((data) => {
				const correctedArtist = data.corrections?.correction?.artist;
				if (correctedArtist) {
					artist = correctedArtist;
				}
			})

			fetch(`https://ws.audioscrobbler.com/2.0/?method=track.getcorrection&artist=${artist}&track=${track}&api_key=ae4f9bac310f0d968cd4f48f80abd386&format=json`)
			.then((res) => res.json())
			.then((data) => {
				const correctedTrack = data.corrections?.correction?.track;
				if (correctedTrack) {
					track = correctedTrack;
				}
			})



			// track search based on track title and artist last.fm api
			fetch(
				`https://ws.audioscrobbler.com/2.0/?method=track.getsimilar&artist=${artist}&track=${track}&api_key=ae4f9bac310f0d968cd4f48f80abd386&format=json`
			)
				.then((res) => res.json())
				.then((data) => {
					console.log(data);
					tracks = data.similartracks?.track?.slice(0, 10) || [];
					isLoading = false;
					isWrong = false;
					isNotFound = tracks.length === 0;
				});
		}  if (artist || track) {
			isWrong = true;
		} else {
			isNotFound = true;
		}
	}

	function handleArtistInput(event) {
		artist = event.target.value;
	}

	function handleTrackInput(event) {
		track = event.target.value;
	}
</script>

<span><h1>what new songs should i listen to next?</h1></span>

<input type="text" on:input={handleArtistInput} placeholder="Enter artist" />
<input type="text" on:input={handleTrackInput} placeholder="Enter song" />
<button on:click={searchTracks}>Search</button>

{#if isLoading}
	<p>Loading...</p>
{:else if isWrong}
	<p>Please enter right values</p>
{:else if isNotFound}
	<p>Cannot find any results</p>
{:else}
	<div>
		<ul>
			{#each tracks as track}
				<li>{track.name} by {track.artist.name}</li>
			{/each}
		</ul>
	</div>
{/if}

<style>
	li {
		list-style-type: none;
		margin-top: 1rem;
	}

	h1 {
		display: block;
	}
</style>
