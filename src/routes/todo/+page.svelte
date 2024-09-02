<script lang="ts">
	import { Checkbox, Button, Input, CloseButton } from 'flowbite-svelte';
	import '../../app.css';

	interface ToDo {
		title: string;
		checked: boolean;
	}

	let toDoList: ToDo[] = [];
	let newToDo: string = '';
	let isButtonDisabled = true;

	$: if (newToDo.length > 0) {
		isButtonDisabled = false;
	} else {
		isButtonDisabled = true;
	}

	const handleToDoButtonClick = () => {
		toDoList = [...toDoList, { title: newToDo, checked: false }];
		newToDo = '';
	};

	$: console.log(toDoList);

	const handleCheckboxClick = (index: number) => {
		console.log(index);
	};

	const handletoDoItemDelete = (index: number) => {
		toDoList = toDoList.filter((toDo, i) => i !== index);
	};
</script>

<div class="flex">
	<div>
		<p>My To Do list:</p>
		{#if toDoList.length === 0}
			<p>No items in the list</p>
		{:else}
			<ul
				class="w-fit bg-white rounded-lg border border-gray-200 dark:bg-gray-800 dark:border-gray-600 divide-y divide-gray-200 dark:divide-gray-600"
			>
				{#each toDoList as toDo, index}
					<li class="flex flex-row items-center justify-between">
						<Checkbox
							class="p-3 w-3/4 "
							bind:checked={toDo.checked}
							on:click={() => (toDo.checked = !toDo.checked)}
							><p class:done={toDo.checked}>{toDo.title}</p></Checkbox
						>
						<CloseButton on:click={() => handletoDoItemDelete(index)} />
					</li>
				{/each}
			</ul>
		{/if}
		<div class="flex flex-row mt-2">
			<Input
				type="text"
				name="to do"
				class="h-8 mr-2"
				bind:value={newToDo}
				placeholder="Add To Do"
			/>
			<Button
				color="green"
				class="h-8 mr-2 w-40"
				on:click={handleToDoButtonClick}
				disabled={isButtonDisabled}>Add to list</Button
			>
		</div>
	</div>
</div>

<style>
	.done {
		text-decoration: line-through;
	}
</style>
