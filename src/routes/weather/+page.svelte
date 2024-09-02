<script lang="ts">
	import { Select, Card, Radio } from 'flowbite-svelte';
	import { europeanCountries } from '../../models/EuropeanCapitals';
	import type { WeatherApiResponse } from '../../models/WeatherApiResponse';
	import '../../app.css';

	let weatherData: WeatherApiResponse;
	let loading = false;
	let units = 'metric';
	let selectedCountry: number[] = [];

	const selectOptions = europeanCountries.map((country) => ({
		name: country.country,
		value: country.coordinates // or any other unique identifier from your EuropeanCountries objects
	}));

	$: console.log(selectedCountry);

	$: lat = selectedCountry[0];
	$: lon = selectedCountry[1];

	$: if (selectedCountry.length > 0) {
		loading = true;
		fetch(
			`https://api.openweathermap.org/data/2.5/weather?lat=${lat}&lon=${lon}&units=${units}&appid=ca61cfce9358253c1cfcd34a23c4ca34`
		)
			.then((response) => response.json())
			.then((data) => {
				weatherData = data;
				loading = false;
			});
	}
</script>

<div class="flex flex-col">
	<div>
		<h2 class="text-blue-700">Please pick a country:</h2>
		<Select class="my-2 w-fit" items={selectOptions} bind:value={selectedCountry} />
	</div>
	<div>
		<div class="flex flex-row">
			<Radio bind:group={units} value="metric">Metric (°C)</Radio>
			<Radio bind:group={units} value="imperial">Imperial (°F)</Radio>
		</div>
	</div>
</div>

<div>
	{#if selectedCountry.length > 0}
		{#if weatherData}
			<Card>
				{#if loading}
					<p>Loading</p>
				{:else}
					<h2 class="mb-2 text-2xl font-bold tracking-tight text-gray-900 dark:text-white">
						Weather in <strong
							>{europeanCountries.find((country) => country.coordinates === selectedCountry)
								?.country}</strong
						>
					</h2>
					<p class="font-normal text-gray-700 dark:text-gray-400 leading-tight">
						Temperature: {Math.round(weatherData.main?.temp)}
					</p>
					<p class="font-normal text-gray-700 dark:text-gray-400 leading-tight">
						Feels like: {Math.round(weatherData.main?.feels_like)}
					</p>
					<p class="font-normal text-gray-700 dark:text-gray-400 leading-tight">
						Humidity: {weatherData.main?.humidity}
					</p>
					<p class="font-normal text-gray-700 dark:text-gray-400 leading-tight">
						Pressure: {weatherData.main?.pressure}
					</p>
				{/if}
			</Card>
		{/if}
	{/if}
</div>
