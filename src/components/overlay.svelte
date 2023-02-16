<script context="module" lang="ts">
	export type Todo = {
		id: number;
		name: string;
		description: string;
		done: boolean;
	};
</script>

<script lang="ts">
	export let overlayWidth: string;
	export let todoArray;
	export let uid;

	// holds input values
	let newTodoName: string = '';
	let newTodoDescription: string = '';

	function handleNewTodo() {
		todoArray.active = [
			...todoArray.active,
			{ id: uid++, name: newTodoName, description: newTodoDescription, done: false }
		];
		newTodoName = '';
		newTodoDescription = '';
		hideOverlay(true);
	}

	function hideOverlay(event: { key: string; keyCode: number } | MouseEvent | boolean) {
		if (event instanceof MouseEvent || typeof event === 'boolean') {
			overlayWidth = '0%';
		} else {
			if (event.key === 'Escape' || event.keyCode === 27) {
				overlayWidth = '0%';
			}
		}
	}
</script>

<div
	id="myNav"
	class="overlay"
	style="width: {overlayWidth}"
	on:click|self={hideOverlay}
	on:keyup={hideOverlay}
>
	<!-- Button to close the overlay navigation -->
	<!-- <a href="javascript:void(0)" class="closebtn" onclick="closeNav()">&times;</a> -->

	<!-- Overlay content -->
	<div class="overlay-content">
		<form class="content">
			<div class="inputLine">
				<p>Description</p>
				<textarea bind:value={newTodoName} />
			</div>

			<!-- <input type="text" bind:value={$user.name} /> -->
			<div class="inputLine">
				<p>Details (optional)</p>
				<textarea bind:value={newTodoDescription} />
			</div>

			<div class="inputLine">
				<button on:click={handleNewTodo}>Submit</button>
			</div>

			<!-- https://svelte.dev/repl/02b5ad4e35ef4aa98181fb290756b9d6?version=3.12.1 -->
		</form>
	</div>
</div>

<style>
	.overlay {
		/* Height & width depends on how you want to reveal the overlay (see JS below) */
		height: 100%;
		position: fixed; /* Stay in place */
		z-index: 1; /* Sit on top */
		left: 0;
		top: 0;
		background-color: rgba(0, 0, 0, 0.479); /* Black w/opacity */
		overflow-x: hidden; /* Disable horizontal scroll */
		transition: 0.5s; /* 0.5 second transition effect to slide in or slide down the overlay (height or width, depending on reveal) */

		text-transform: uppercase;

		display: inline-flex;
		align-items: center;
		justify-content: space-around;
	}

	/* Position the content inside the overlay */
	.overlay-content {
		background-color: rgb(94, 92, 91);
		/* position: relative; */

		border-radius: 10px;

		max-width: 910px;
		min-width: 910px;
		box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.404);
	}

	/* https://www.w3schools.com/howto/howto_js_fullscreen_overlay.asp */

	.overlay-content .inputLine {
		width: 100%;
		padding: 0px 20px;
		margin: 1px 0;
		box-sizing: border-box;

		/* float: right; */
	}

	.overlay-content .inputLine p {
		color: #ffffff;
		margin: 10px;
	}

	.overlay-content .inputLine textarea {
		width: 100%;
		resize: none;
		border-radius: 10px;

		font-size: 18px;
		padding: 12px 12px;
		box-sizing: border-box;
		box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.404);
	}

	.overlay-content .inputLine button {
		float: right;
		width: 30%;
		margin-top: 40px;
		margin-bottom: 20px;
		padding: 12px 20px;
		border-radius: 10px;
		font-size: large;
		background-color: rgba(229, 46, 46, 0.4);
		border: none;
		transition: all 0.3s ease 0s;
		box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.404);
	}

	.overlay-content .inputLine button:hover {
		background-color: #e52e2e;
		box-shadow: 0px 15px 20px rgba(229, 46, 46, 0.4);
		color: #fff;
		transform: translateY(-7px);
	}
</style>
