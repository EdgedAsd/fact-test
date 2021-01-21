<template>
	<div class="wrapper">
		<div class="selection-wrapper" v-if="selections.length > 0">
			<Selections
				v-bind:selections="selections"
			/>
		</div>
		<p v-else-if="!error">Loading...</p>
		<p v-else>Error!</p>
	</div>
</template>

<script>

import Selections from './components/Selections';

// Распаковка вложенных массивов
function joinArray(array, count = 0) {
	if (count < array.length) {
		if (Array.isArray(array[count])) {
			array.splice(count, 1, ...array[count]);
			return joinArray(array, count)
		}
		else {
			return joinArray(array, count+1)
		}
	}
}

// Разделение по типам, undefined и null не учитывать, как в ТЗ в примере
function splitTypes(array) {
	let types = [];
	for (let elem of array) {
		let type = typeof(elem);
		if ((types.indexOf(type) === -1) && (elem !== undefined) && (elem !== null)) {
			types.push(type);
		}
	}
	
	let newArray = [];
	for (let type of types) {
		let values = array.filter(elem => ((typeof(elem) === type) && (elem !== null)));
		newArray.push(values);
	}

	return newArray;
}

export default {
	name: 'app',
	components: {
		Selections
	},
	data() {
		return {
			selections: [],		// Массив массивов, разделенных по типу
			error: false		// Флаг ошибки
		}
	},
	mounted() {
		fetch('https://raw.githubusercontent.com/WilliamRu/TestAPI/master/db.json')
			.then(response => (response.ok) ? response.json() : Promise.reject(response))
			.then(data => {
				let testArr = data[Object.keys(data)[0]];
				joinArray(testArr);
				this.selections = splitTypes(testArr);
			})
			.catch(() => this.error = true)
	}
}

</script>

<style lang="scss">

	* {
		padding: 0;
		margin: 0;
	}

	html {
		width: 100%;
		height: 100%;

		body {
			width: 100%;
			height: 100%;

			ol, ul {
				list-style: none;
			}
		}
	}

</style>
