<script>
export default {
	name: 'app-button',
};
</script>

<template>
	<button>
		<slot></slot>
	</button>
</template>

<style scoped>
button {
	border: 2px solid rgb(0, 221, 255);
	box-shadow: 0 0 8px 0 rgb(0, 221, 255);
	padding: 10px;
	border-radius: 10px;
	background: none;
	cursor: pointer;
}

button:hover {
	background-color: rgba(0, 0, 0, 0.25);
}
</style>
