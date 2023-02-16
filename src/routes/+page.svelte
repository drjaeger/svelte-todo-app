<script lang="ts">
	import AddButton from '../components/newTodoButton.svelte';
	import Overlay, { type Todo } from '../components/overlay.svelte';
	import TodoLi from '../components/listItem.svelte';
	import { flip } from 'svelte/animate';

	export let inTranMap = new Map();
	export let outTranMap = new Map();

	let overlayWidth: string;

	let uid = 1;

	let allTodos: { active: Todo[]; completed: Todo[] } = {
		active: [
			{ id: uid++, name: '2: 26 characters xoxoxoxoxoxo', description: '', done: false },
			{ id: uid++, name: '3: 24charactersxoxoxoxoxoxo', description: '', done: false },
			{ id: uid++, name: '4: Watch a movie Akropole', description: '', done: false },
			{ id: uid++, name: '5: Play Valheim', description: '', done: false }
		],
		completed: [
			{ id: uid++, name: '1: 13 characters', description: '', done: true },

			{ id: uid++, name: '6: 13 characters', description: '', done: true },
			{ id: uid++, name: '7: 13 characters', description: '', done: true }
		]
	};

	const today = new Date().getDate();
	console.log(today);
	const points = [1, today, 4, 21, 30];

	//TODO for todos
	// - move todo's to https://svelte.dev/tutorial/writable-stores
	// - add db or server support
	// - wtf is this: https://www.youtube.com/watch?v=3ygAZLEnG7I&ab_channel=OnlineTutorials
	// done / undone / edit / delete buttons
	// timeline
	// smooth ul animation when row added
	// at least 1 character of descriptio required
	// dont allow click while in transition
</script>

<!-- https://stackoverflow.com/questions/46044589/dynamically-resize-columns-in-css-grid-layout-with-mouse -->

<body>
	{#each [allTodos.active, allTodos.completed] as block, index (index)}
		<!-- todo: implement smooth height transition when new line added (look into tweened()) -->
		<ul class="todo-list" id="todoLists">
			{#each block as todo (todo.id)}
				<div class="liContainer" animate:flip={{ duration: 1000 }}>
					<TodoLi bind:inTranMap bind:outTranMap bind:allTodos bind:todoInfo={todo} />
				</div>
			{/each}
		</ul>
	{/each}

	<AddButton bind:overlayWidth />
	<Overlay bind:overlayWidth bind:todoArray={allTodos} bind:uid />
	<!-- todo: move -->
	<svg>
		<line x1="0" y1="80" x2="938" y2="80" stroke="grey" stroke-width="4px" />
		{#each points as point}
			<!-- streamline calculations (not all days have 30days!) -->
			<circle cx={((940 - 0) / 30) * (point - 1) + 15} cy="80" r="15" />
		{/each}
	</svg>
	<!--  -->
</body>

<style>
	/* todo: move */
	svg {
		width: 100%;
	}
	circle {
		fill: rgb(255, 255, 255);
	}
	circle:hover {
		fill: red;
		/* do smthin nice here */
	}
	/*  */
	:root {
		--max-min-page-width: 940px;
	}
	body {
		background-color: #36393f;
		min-height: 600px;
		max-width: var(--max-min-page-width);
		min-width: var(--max-min-page-width);

		margin: 0 auto;
		font-family: 'Roboto', sans-serif;
	}
	.todo-list {
		color: #d4d9eb;
		font-size: 2em;
		background-color: #36393f;
		border-radius: 10px;
		padding: 10px;

		display: grid;
		grid-template-columns: repeat(3, 305px);
	}
</style>
