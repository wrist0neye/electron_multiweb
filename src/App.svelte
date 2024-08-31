<script>
	// import DragTest from './Components/DragTest.svelte';
	import WindowOne from './Components/WindowOne.svelte'
	export let name;

</script>

<style>
	html{
		width: 100%;
		height: 100%;
		padding:0px;
		margin: 0px;
	}
	body{
		width:100%;
		height:100%;
		padding:0px;
		margin: 0px;
	}
	main {
		position: relative;

		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
		height:100%;
		padding:10px 10px;
	}
	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}
	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>

<main>
	<h1>Hello {name}!</h1>
	<p>Visit the <a href="https://svelte.dev/tutorial">Svelte tutorial</a> to learn how to build Svelte apps.</p>
	<br/>
	<!-- <DragTest></DragTest> -->
	<WindowOne></WindowOne>
</main>