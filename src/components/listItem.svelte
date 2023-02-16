<script lang="ts">
	import type { Todo } from './overlay.svelte';
	import { fade, crossfade } from 'svelte/transition';

	export let inTranMap = new Map();
	export let outTranMap = new Map();
	export let todoInfo: Todo;
	export let allTodos: { active: Todo[]; completed: Todo[] };

	type FadeParams = {
		key: number;
	};

	const transitionTime = 300;
	let transitionActive = false;

	function handleDone(todo: Todo) {
		if (transitionActive) return;
		let completed = allTodos.completed;
		let active = allTodos.active;
		// todo: make neater
		if (todo.done) {
			completed = completed.filter((t) => t !== todo);
			todo.done = !todo.done;
			active = [...active, todo];
		} else {
			active = active.filter((t) => t !== todo);
			todo.done = !todo.done;
			completed = [...completed, todo];
		}
		allTodos = { active, completed };
		transitionActive = true;
		// todo: update to event based enable disable
		// https://svelte.dev/tutorial/transition-events
		setTimeout(() => {
			transitionActive = false;
		}, transitionTime);
	}

	function coordinatedTransition(myNode: Element, otherRect: { left: number; top: number }) {
		// crossFade gets the bounding rect of both nodes and tweens between them here.
		let myRect = myNode.getBoundingClientRect();

		const style = getComputedStyle(myNode);
		const transform = style.transform === 'none' ? '' : style.transform;

		let deltaX = otherRect.left - myRect.left;
		let deltaY = otherRect.top - myRect.top;
		return {
			duration: transitionTime,
			css: (t: any, u: number) => {
				let x = deltaX * u;
				let y = deltaY * u;
				let style = `transform-origin:top left;
					transform: ${transform} translate(${x}px, ${y}px);
					opacity:${t}`;
				return style;
			}
		};
	}

	function makeCrossfade(
		node: Element,
		params: FadeParams,
		mine: Map<any, any>,
		other: Map<any, any>
	) {
		// Add our node to the Map.
		// addMessage(description, params.key, 'initializing');
		mine.set(params.key, node.getBoundingClientRect());
		return function run() {
			// addMessage(description, params.key, 'starting transition');
			// Now all the nodes have initialized.
			// See if there is another node transitioning with the same key.
			let matchingRect = other.get(params.key);
			// Clean up. We don't delete our own entry because we don't know if the other side has used it yet.
			other.delete(params.key);

			if (matchingRect) {
				// addMessage(description, params.key, 'creating coordinated transition');
				// We have a matching pair, so transition them together.
				return coordinatedTransition(node, matchingRect);
			} else {
				// addMessage(description, params.key, 'no matching element');
				// No matching element for this one, so do something else. Crossfade calls this the fallback.
				mine.delete(params.key);
				return fade(node);
			}
		};
	}

	const [inTran, outTran] = [
		(node: Element, params: { key: number }) => makeCrossfade(node, params, inTranMap, outTranMap),
		(node: Element, params: { key: number }) => makeCrossfade(node, params, outTranMap, inTranMap)
	];
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<li
	class="todo-item-{todoInfo.done}"
	id="todo-item"
	on:click={() => handleDone(todoInfo)}
	style="opacity:{todoInfo.done ? '50%' : '100%'}"
	in:inTran={{ key: todoInfo.id }}
	out:outTran={{ key: todoInfo.id }}
>
	{todoInfo.name}
	<div class="done-indicator" style="opacity:{todoInfo.done ? '100%' : '0%'}" />
</li>

<style>
	:root {
		--red-ribbon: 29px solid red;
		--white-ribbon: 29px solid rgba(238, 238, 238, 0);
		--ribbon-transition: border 0.1s, opacity 0.5s, visibility 0.5s;
	}
	.done-indicator {
		position: absolute;
		bottom: 0;
		left: 0;
		content: '';
		position: absolute;
		bottom: 0;
		border-top: var(--red-ribbon);
		border-left: var(--white-ribbon);
		transition: var(--ribbon-transition);
	}
	.done-indicator:before {
		content: '';
		position: absolute;
		left: -29px;
		bottom: 29px;
		border-bottom: var(--red-ribbon);
		border-right: var(--white-ribbon);
		transition: var(--ribbon-transition);
	}
	.done-indicator:after {
		content: '';
		position: absolute;
		bottom: 0;
		border-top: var(--white-ribbon);
		border-left: var(--red-ribbon);
		transition: var(--ribbon-transition);
	}

	#todo-item {
		cursor: default;
		background-color: #525458;
		padding: 10px;
		border-radius: 10px;
		width: 260px;
		height: 120px;
		text-align: center;
		margin: 10px;

		font-family: 'Roboto', sans-serif;
		text-transform: uppercase;
		font-size: medium;

		display: inline-flex;
		align-items: center;
		justify-content: space-around;
		transition: all 0.3s ease 0s;
		box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.404);

		position: relative;
	}

	#todo-item:hover {
		background-color: #e52e2e;
		box-shadow: 0px 15px 20px rgba(229, 46, 46, 0.4);
		color: #fff;
		transform: translateY(-7px);

		--red-ribbon: 29px solid #36393f;
	}
</style>
