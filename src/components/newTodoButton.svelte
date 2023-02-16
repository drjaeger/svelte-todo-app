<script lang="ts">
	export let overlayWidth = '0%';
	function handleClick() {
		overlayWidth = '100%';
	}
</script>

<button class="add-button" on:click={handleClick}> + </button>

<style>
	.add-button {
		background-color: #515357;
		border: none;
		color: white;

		height: 100px;
		width: 100px;
		font-size: 100px;
		border-radius: 100px;
		position: absolute;
		right: 50px;
		bottom: 50px;
		display: inline-flex;
		align-items: center;
		justify-content: space-around;
		transition: all 0.3s ease 0s;
		box-shadow: 0px 8px 15px rgba(0, 0, 0, 0.404);
	}
	.add-button:hover {
		background-color: #e52e2e;
		box-shadow: 0px 15px 20px rgba(229, 46, 46, 0.4);
		transform: translateY(-7px);
	}
</style>
