<script lang="ts">
	import { onMount } from 'svelte';
	import { europeanCountries } from '../../models/EuropeanCapitals';
	import type { WeatherApiResponse } from '../../models/WeatherApiResponse';
	import '../../app.css';

	let weatherData: WeatherApiResponse;
	let loading = false;
	let units = 'metric';
	let selectedCountry: number[] = [];

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

<div>
	<a class="underline text-yellow-800" href="/">Go home</a>
	<h2 class="text-blue-300">Please pick a country:</h2>
	<select bind:value={selectedCountry}>
		<option value="Choose a country">Choose a country</option>
		{#each europeanCountries as country (country)}
			<option value={country.coordinates}>{country.country}</option>
		{/each}
	</select>
</div>

<div>
	{#if loading}
		<p>Loading</p>
	{/if}
	{#if selectedCountry.length > 0}
		<div>
			<label>
				<input type="radio" bind:group={units} value="metric" /> Metric (°C)
			</label>
			<label>
				<input type="radio" bind:group={units} value="imperial" /> Imperial (°F)
			</label>
		</div>
		<h2 class="text-blue-500">
			Weather in {europeanCountries.find((country) => country.coordinates === selectedCountry)
				?.country}
		</h2>
		{#if weatherData}
			<p>Temperature: {weatherData.main?.temp}</p>
			<p>Feels like: {weatherData.main?.feels_like}</p>
			<p>Humidity: {weatherData.main?.humidity}</p>
			<p>Pressure: {weatherData.main?.pressure}</p>
		{/if}
	{/if}
</div>
