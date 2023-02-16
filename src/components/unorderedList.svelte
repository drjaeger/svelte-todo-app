<!-- <script lang="ts">
	import TodoLi from './listItem.svelte';
	import type { Todo } from './overlay.svelte';

	export let todoArray: Array<Todo>;
	export let id = '0';
</script>

<ul class="todo-list-{id}" id="todoLists">
	{#each todoArray as todo (todo.id)}
		<TodoLi bind:todoArray bind:todoInfo={todo} />
	{/each}
</ul>

<style>
	#todoLists {
		color: #d4d9eb;
		font-size: 2em;
		background-color: #36393f;
		border-radius: 10px;
		padding: 10px;

		display: grid;
		grid-template-columns: repeat(3, 305px);
	}
</style> -->
