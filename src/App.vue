<template>
	<div id="app">
		<HelloWorld />
	</div>
</template>

<script>
	import HelloWorld from "./components/HelloWorld.vue";

	export default {
		name: "App",
		components: {
			HelloWorld,
		},
	};
</script>

<style>
	* {
		margin: 0;
		padding: 0;
		box-sizing: border-box;
		overflow: hidden;
	}
	#app {
		font-family: Avenir, Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		text-align: center;
		color: #000;
	}
</style>
