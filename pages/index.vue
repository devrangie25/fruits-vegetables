<template>
	<div class="row">
		<div class="column">
			<List
				:items="fruitsArr"
				title="fruits"
				@new-sorted-items="newSortedItems"
			/>
		</div>
		<div class="column">
			<List
				:items="vegetablesArr"
				title="vegetables"
				@new-sorted-items="newSortedItems"
			/>
		</div>
	</div>
</template>

<script>
export default {
	name: "list-page",
	layout: "default",

	data() {
		return {
			items: [
				{
					name: 'Apple',
					type: 'Fruits'
				},
				{
					name: 'Banana',
					type: 'Fruits'
				},
				{
					name: 'Orange',
					type: 'Fruits'
				},
				{
					name: 'Guava',
					type: 'Fruits'
				},
				{
					name: 'Cherry',
					type: 'Fruits'
				},
				{
					name: 'Carrots',
					type: 'Vegetables'
				},
				{
					name: 'Cabbage',
					type: 'Vegetables'
				},
				{
					name: 'Squash',
					type: 'Vegetables'
				},
				{
					name: 'Eggplant',
					type: 'Vegetables'
				},
				{
					name: 'Tomatoes',
					type: 'Vegetables'
				}
			],
			tempFruitsArr: [],
			tempVegesArr: [],
			isNewTempFruitsArr: false
		};
	},

	computed: {
		fruitsArr() {
			return this.items.map((item, ind) => {
				if (item.type === 'Fruits') {
					return { name : item.name, type: item.type.toLowerCase(), order: ind + 1}
				}
			}).filter(val => val !== undefined)
		},

		vegetablesArr() {
			return this.items.map((item, ind) => {
				if (item.type === 'Vegetables') {
					return { name : item.name, type: item.type.toLowerCase(), order: ind + 1}
				}
			}).filter(val => val !== undefined)
		},
	},

	methods: {

		finalizeItemsArr(params) {
			const newArr = {
				1 : [...this.tempFruitsArr, ...this.tempVegesArr],
				2 : [...this.tempFruitsArr, ...this.vegetablesArr],
				3 : [...this.tempVegesArr, ...this.fruitsArr]
			}
			return newArr[params] ?? []
		},

		newSortedItems(items, title) {
			if (title === 'Fruits') {
				this.tempFruitsArr = items.map((item, ind) => {
					return { name: item.name, order: ind + 1, type: item.type}
				})
			} else {
				this.tempVegesArr = items.map((item, ind) => {
					return { name: item.name, order: ind + 1, type: item.type}
				})
			}

			if (this.tempFruitsArr.length > 0 && this.tempVegesArr.length > 0) {
				this.items = this.finalizeItemsArr(1)
			}

			if (this.tempFruitsArr.length > 0 && this.tempVegesArr.length === 0) {
				this.items = this.finalizeItemsArr(2)
			}

			if (this.tempVegesArr.length > 0 && this.tempFruitsArr.length === 0 ) {
				this.items = this.finalizeItemsArr(3)
			}

			console.log('NEW ITEMS ARRAY SORTED', this.items)
		},
	},
};
</script>
