<template>
	<div class="wrapper">
		<h1>Forecast App</h1>
		<p>
			See the forecast of
			<span>{{ city === '' ? 'your city' : cityName }}</span>
		</p>
		<input
			type="text"
			v-model="city"
			name="forecast"
			id="forecast-input"
			placeholder="City" />
		<button
			v-show="city.length"
			@click="getWeather()">
			Submit
		</button>
		<p class="error">{{ error }}</p>

		<div v-if="!!this.info">
			<p>{{ showTemp }}</p>
			<p>{{ showFeelsLike }}</p>
			<p>{{ showMinTemp }}</p>
			<p>{{ showMaxTemp }}</p>
		</div>
	</div>
</template>

<script>
	import axios from 'axios';

	export default {
		data() {
			return {
				city: '',
				error: '',
				info: null,
			};
		},
		computed: {
			cityName() {
				return this.city;
			},
			showTemp() {
				return 'Температура: ' + Math.floor(this.info.main.temp) + '°C';
			},
			showFeelsLike() {
				return 'Ощущается как: ' + Math.floor(this.info.main.feels_like) + '°C';
			},
			showMinTemp() {
				return (
					'Минимальная температура сегодня: ' +
					Math.floor(this.info.main.temp_min) +
					'°C'
				);
			},
			showMaxTemp() {
				return (
					'Максимальная температура сегодня: ' +
					Math.floor(this.info.main.temp_max) +
					'°C'
				);
			},
		},
		methods: {
			getWeather() {
				if (this.city.trim().length < 2) {
					this.error = "City's name must be over than 2 symbols";
					return false;
				}
				this.error = '';
				axios
					.get(
						`https://api.openweathermap.org/data/2.5/weather?q=${this.city}&units=metric&appid=825a68baa3a4615072738030317fc7d6`
					)
					.then((res) => (this.info = res.data));
			},
		},
	};
</script>

<style scoped>
	.error {
		color: rgb(239, 78, 88);
	}
	.wrapper {
		width: 900px;
		height: 500px;
		border-radius: 50px;
		padding: 20px;
		text-align: center;
		font-size: 25px;
		background-color: rgb(76, 50, 248, 0.4);
		font-family: 'Gill Sans', 'Gill Sans MT', 'Calibri', 'Trebuchet MS',
			'sans-serif';
	}

	.wrapper h1 {
		margin-top: 10px;
		font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
	}
	.wrapper p {
		font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
	}
	.wrapper input {
		margin-top: 20px;
		border-radius: 4px;
		border: 1px solid rgb(225, 230, 236);
		background-color: transparent;
		padding: 2px;
		outline: none;
		width: 15rem;
		height: 2rem;
	}
	::placeholder {
		color: rgb(83, 83, 83);
		font-family: Verdana, Geneva, Tahoma, sans-serif;
		font-size: 17px;
		padding-left: 4px;
	}
	.wrapper button {
		background: transparent;
		border: 1px solid rgb(225, 230, 236);
		cursor: pointer;
		padding: 0px 2px;
		color: rgb(83, 83, 83);
		height: 21.5px;
		border-radius: 5px;
		box-sizing: border-box;
		margin-left: 3px;
		transition: transform 400ms ease;
		height: 2rem;
		width: 4rem;
	}
	.wrapper input:focus {
		border-color: rgb(242, 247, 255);
	}
	.wrapper button:hover {
		transform: scale(1.05);
	}
	span {
		color: red;
		font-family: 'Franklin Gothic Medium', 'Arial Narrow', Arial, sans-serif;
		font-size: 30px;
	}
</style>
