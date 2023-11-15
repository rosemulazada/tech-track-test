<!-- src/routes/+page.svelte -->

<script>
	import SimilarTracks from '../components/similarTracks.svelte'; // Import the BarChart component
	import UserData from '../components/UserData.svelte';
	import '../components/global.css';
	import TopTags from '../components/TopTags.svelte';
</script>

<header>
	<!-- Header content -->
</header>

<body>
	<main>
		<!-- Include the BarChart component here -->

		<UserData />
		<TopTags />
		<!-- <SimilarTracks /> -->
	</main>
</body>

<footer>
	<!-- Footer content -->
</footer>

<style>
</style>
