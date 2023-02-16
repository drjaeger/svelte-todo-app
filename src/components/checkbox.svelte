<script lang="ts">
	export let checked = false;
</script>

<input class="spade-checkbox" type="checkbox" bind:checked />

<style>
	.spade-checkbox {
		appearance: none;
		background-color: #fafafa;
		border: 1px solid #cacece;
		/* box-shadow: 0 1px 2px rgba(0, 0, 0, 0.603), inset 0px -15px 10px -12px rgba(0, 0, 0, 0.05); */
		padding: 11px;
		border-radius: 5px;
		display: inline-block;
		position: relative;
	}

	.spade-checkbox:checked:after {
		content: '\2660';
		font-size: 30px;
		position: absolute;
		top: -7px;
		left: 3px;
		color: rgb(0, 0, 0);
	}
</style>
