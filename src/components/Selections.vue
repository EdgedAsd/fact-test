<template>
	<div>
		<ul class="selection-list">
			<Selection
				v-for="(selection,index) in selections"
				v-bind:selection="selection"
				v-bind:type="typeof selection[0]"
				v-bind:values="currentState[typeof(selection[0])]"
				:key="index"
				@change-value="changeValue"
			/>
		</ul>
		<div class="panel">
			<button class="btn" :disabled="prevStates.length === 0" @click="getPrevState">Prev</button>
			<button class="btn __center" @click="resetStates">Reset</button>
			<button class="btn" :disabled="nextStates.length === 0" @click="getNextState">Next</button>
		</div>
	</div>
</template>

<script>
	
	import Selection from './Selection';

	export default {
		components: {
			Selection
		},
		props: {
			selections: Array
		},
		data() {
			return {
				prevStates: [],		// Массив предыдущих состояний
				nextStates: [],		// Массив последующих состояний
				currentState: {}	// Объект текущего состояния
			}
		},
		methods: {
			changeValue(type, values) {
				if (this.prevStates.length === 10) {
					this.prevStates.splice(0,1);
				}
				this.prevStates.push(JSON.parse(JSON.stringify(this.currentState)));
				this.currentState[type] = values;
				this.nextStates = [];
			},
			getPrevState() {
				let state = this.prevStates[this.prevStates.length-1];
				this.prevStates.splice(this.prevStates.length-1,1);
				this.nextStates.push(this.currentState);
				this.currentState = state;
			},
			getNextState() {
				let state = this.nextStates[this.nextStates.length-1];
				this.nextStates.splice(this.nextStates.length-1,1);
				this.prevStates.push(this.currentState);
				this.currentState = state;
			},
			resetStates() {
				this.prevStates = [];
				this.nextStates = [];
				this.currentState = {};
				for (let type of this.types) {
					this.currentState[type] = [];
				}
			}
		},
		computed: {
			types() {
				let types = [];
				for (let select of this.selections) {
					types.push(typeof select[0]);
				}
				return types;
			}
		},
		mounted() {
			for (let type of this.types) {
				this.currentState[type] = [];
			}
		}
	}

</script>

<style lang="scss" scoped>

	.selection-list {
		display: flex;
		width: 84%;
		margin: auto;
		justify-content: space-between;
		padding-top: 100px;
	}

	.panel {
		width: 84%;
		text-align: center;
		margin: auto;
		margin-top: 50px;

		.btn {
			width: 80px;
			height: 30px;
			cursor: pointer;
		}

		.__center {
			margin: 0 30px;
		}
	}

</style>