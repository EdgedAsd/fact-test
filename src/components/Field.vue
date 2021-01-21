<template>
	<ul class="field">
		<li class="field-item" v-for="(value, index) of values" :key="index">{{value.toString()}}</li>
	</ul>
</template>

<script>
	export default {
		props: {
			values: Array
		}
	}
</script>

<style lang="scss" scoped>

	.field {
		height: 300px;
		background-color: #fff;
		margin-top: 50px;
		border-radius: 15px;
		box-shadow: inset -1px -1px 6px 2px rgba(50,50,50,.3), 0 0 3px 1px #333;
		padding: 20px;
		box-sizing: border-box;

		.field-item {
			font-size: 20px;
			margin-top: 10px;
		}
	}

</style>