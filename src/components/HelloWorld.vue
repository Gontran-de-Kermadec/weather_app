<template>
	<div class="container default">
		<div class="land_page">
			<div class="bloc one">
				<p>UNE</p>
			</div>
			<div class="bloc two">
				<p>SUPER</p>
			</div>
			<div class="bloc three">
				<p>APPLI</p>
			</div>
			<div class="bloc four">
				<p>MÉTÉO</p>
			</div>
		</div>
		<header>
			<h1>Recherchez votre météo</h1>
			<input type="text" name="search" id="search" v-model="research" />
			<button @click="getData()">
				Chercher
			</button>
		</header>

		<div class="city">
			<h2>
				{{ city }}, <span>{{ country }}</span>
			</h2>
			<div class="weather">
				<p>{{ weather }}</p>
				<p>{{ temperature }}°</p>
			</div>
			<div class="min_max">
				<div class="min">
					<p>Min</p>
					<p>{{ min }}°</p>
				</div>
				<div class="max">
					<p>Max</p>
					<p>{{ max }}°</p>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	//const axios = require("axios");
	import { gsap } from "gsap";

	export default {
		name: "HelloWorld",
		data() {
			return {
				research: "",
				city: "",
				country: "",
				temperature: "",
				min: "",
				max: "",
				humidity: "",
				weather: "",
			};
		},
		methods: {
			async getData() {
				console.log(this.research);
				// const data = await axios.get(
				// 	"http://api.openweathermap.org/data/2.5/weather?q=paris&units=metric&appid=66e6b8b8222e37b4274479f11e417441"
				// );
				//window.fetch("http://api.openweathermap.org/data/2.5/weather?q=paris&units=metric&appid=66e6b8b8222e37b4274479f11e417441")
				const result = await fetch(
					`http://api.openweathermap.org/data/2.5/weather?q=${this.research}&units=metric&appid=66e6b8b8222e37b4274479f11e417441&lang=fr`
				);
				const data = await result.json();
				this.city = data.name;
				this.country = data.sys.country;
				this.temperature = data.main.temp;
				this.min = data.main.temp_min;
				this.max = data.main.temp_max;
				this.weather = data.weather[0].description;
				console.log(data.weather[0].main);
				this.bgImage(data.weather[0].main);
				this.animContent(data.weather[0].main);
			},
			animLandPage() {
				gsap.to(".one", { y: -200, duration: 2, delay: 0.5 });
				gsap.to(".two", { left: "-100%", duration: 2, delay: 0.8 });
				gsap.to(".three", { left: "100%", duration: 2, delay: 1.1 });
				gsap.to(".four", { y: 200, duration: 2, delay: 1.4 });
			},
			animHeader() {
				const tl = gsap.timeline({ delay: 2 });
				tl.fromTo(
					"h1",
					{ opacity: 0, y: -100 },
					{ opacity: 1, y: 0, duration: 1 }
				);
				tl.fromTo("input", { scale: 0 }, { scale: 1, duration: 0.5 });
			},
			animContent(weather) {
				const timeLine = gsap.timeline();
				if (weather === "Clear") {
					timeLine.to(".clear", { opacity: 1, duration: 0.5 });
					timeLine.to("h2", { y: 0, duration: 1 });
					timeLine.to(".weather", { opacity: 1, duration: 0.5 });
					timeLine.to(".min", { x: 0, duration: 0.5 });
					timeLine.to(".max", { x: 0, duration: 0.5 });
				} else if (weather === "Rain") {
					timeLine.to(".rain", { opacity: 1, duration: 0.5 });
					timeLine.to("h2", { y: 0, duration: 1 });
					timeLine.to(".weather", { opacity: 1, duration: 0.5 });
					timeLine.to(".min", { x: 0, duration: 0.5 });
					timeLine.to(".max", { x: 0, duration: 0.5 });
				} else if (weather === "Cloud") {
					timeLine.to(".cloud", { opacity: 1, duration: 0.5 });
					timeLine.to("h2", { y: 0, duration: 1 });
					timeLine.to(".weather", { opacity: 1, duration: 0.5 });
					timeLine.to(".min", { x: 0, duration: 0.5 });
					timeLine.to(".max", { x: 0, duration: 0.5 });
				}
				// timeLine.to("h2", { y: 0, duration: 1 });
				// timeLine.to(".weather", { opacity: 1, duration: 0.5 });
				// timeLine.to(".min", { x: 0, duration: 0.5 });
				// timeLine.to(".max", { x: 0, duration: 0.5 });
			},
			bgImage(weather) {
				let container = document.querySelector(".container");
				if (weather === "Clear") {
					container.classList.remove("rain", "cloud", "default");
					container.classList.add("clear");
				} else if (weather === "Rain") {
					container.classList.remove("clear", "cloud", "default");
					container.classList.add("rain");
				} else if (weather === "Clouds") {
					container.classList.remove("clear", "rain", "default");
					container.classList.add("cloud");
				}
			},
		},
		mounted() {
			this.animLandPage();
			this.animHeader();
		},
	};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
	.container {
		height: 100vh;
	}
	.default {
		background: chocolate;
	}
	.clear {
		background: url("../assets/sunny.jpg") no-repeat 50% 50%;
		background-size: cover;
		opacity: 0;
	}
	.rain {
		background: url("../assets/raindrop.jpg") no-repeat 50% 50%;
		background-size: cover;
		opacity: 0;
		/* color: black; */
	}
	.cloud {
		background: url("../assets/cloud.jpg") no-repeat 50% 50%;
		background-size: cover;
		opacity: 0;
		/* color: black; */
	}
	header {
		padding: 50px 0 30px 0;
		/* background: linear-gradient(to right, #a8c0ff, #3f2b96); */
	}
	h1 {
		margin: 10px 0;
		font-size: 3.5rem;
	}
	input {
		width: 300px;
		font-size: 1.5rem;
		padding: 5px 0;
	}
	.city {
		height: 80vh;
	}
	.city h2 {
		font-size: 2.5rem;
		transform: translateY(800px);
	}
	.weather {
		font-size: 1.2rem;
		opacity: 0;
	}
	.min_max {
		display: flex;
		justify-content: space-around;
		font-size: 1.5rem;
	}
	.min {
		transform: translateX(-600px);
	}
	.max {
		transform: translateX(600px);
	}
	/* .container {
		overflow: hidden;
	} */
	/* .land_page {
		position: absolute;
		width: 100%;
		height: 100vh;
		background: coral;
	} */
	.bloc {
		position: absolute;
		z-index: 100;
		width: 100%;
		height: 25vh;
	}
	.bloc p {
		position: relative;
		top: 50%;
		transform: translateY(-50%);
		font-weight: 900;
		letter-spacing: 10px;
		font-size: 2rem;
	}
	.bloc.one {
		background: #4774c2;
	}
	.bloc.two {
		background: #668bcc;
		top: 25vh;
	}
	.bloc.three {
		background: #85a3d6;
		top: 50vh;
	}
	.bloc.four {
		background: #a3bae0;
		top: 75vh;
	}
	h3 {
		margin: 40px 0 0;
	}
	ul {
		list-style-type: none;
		padding: 0;
	}
	li {
		display: inline-block;
		margin: 0 10px;
	}
	a {
		color: #42b983;
	}
</style>
