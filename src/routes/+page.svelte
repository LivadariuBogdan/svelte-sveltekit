<script>
	import '../app.css';
	import { A } from 'flowbite-svelte';
</script>

<main>
	<div>
		<h1>Links</h1>
	</div>
	<ul>
		<li><A href="/weather" class="font-medium hover:underline">Check out the weather</A></li>
		<li>
			<A href="/authentication" class="font-medium hover:underline">Go to authentication page</A>
		</li>
		<li><A href="/todo" class="font-medium hover:underline">Your To Do list</A></li>
	</ul>
</main>

<style>
</style>
