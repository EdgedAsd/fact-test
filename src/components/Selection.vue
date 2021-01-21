<template>
	<li class="selection">
		<Multiselect
			v-model="value"
			mode="multiple"
			:searchable="false"
			:placeholder="type"
			:options="optionsToString"
			@change="changeValue"
		/>
		<Field
			v-bind:values="values"
		/>
	</li>
</template>

<script>
	// Не использован рекомендуемый multiselect, т. к. возникла ошибка, прогуглить не получилось, в док-ции ничего не было
	// Думаю, проблема в несовместимости версий, поэтому на гитхабе нашел другую библиотеку
	import Multiselect from '@vueform/multiselect';
	import Field from './Field';

	export default {
		props: {
			selection: Array,
			type: String,
			values: Array
		},
		components: {
			Multiselect,
			Field
		},
		data() {
			return {
				value: []
			}
		},
		computed: {
			optionsToString() {
				if (this.type === 'object') {
					let stringOptions = [];
					for (let option in this.selection) {
						stringOptions.push(JSON.stringify(this.selection[option]));
					}

					return stringOptions 
				}
				else {
					return this.selection
				}
			}
		},
		methods: {
			changeValue(value) {
				let newValues = [];
				for (let num of value) {
					(this.type === 'object') ? newValues.push(JSON.stringify(this.selection[num])) : newValues.push(this.selection[num])
				}
				this.$emit('change-value', this.type, newValues)
			}
		}
	}
</script>


<style src="@vueform/multiselect/themes/default.css"></style>

<style lang="scss" scoped>

	.selection {
		width: 30%;
	}

</style>