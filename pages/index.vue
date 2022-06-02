<template>
	<div class="row">
		<div class="column">
			<List :items="fruitsArr" title="fruits" @new-sorted-items="newSortedItems"/>
		</div>
		<div class="column">
			<List :items="vegetablesArr" title="vegetables" @new-sorted-items="newSortedItems"/>
		</div>
	</div>
</template>

<script>
import draggable from "vuedraggable";

export default {
	name: "list-page",
	layout: 'default',
	components: {
		draggable,
	},

	data() {
		return {
			fruits: ["Apple", "Banana", "Orange", "Guava", "Cherry"],
			vegetables: ["Carrots", "Cabbage", "Squash", "Eggplant", "Tomatoes"],
		};
	},

	computed: {
		fruitsArr() {
			return this.fruits.map((name, ind) => {
				return { name, order: ind + 1}
			})
		},

		vegetablesArr() {
			return this.vegetables.map((name, ind) => {
				return { name, order: ind + 1}
			})
		},
	},

	methods: {
		newSortedItems(items, listName) {
			if (listName === 'Vegetables') {
				this.vegetables = items
			} else {
				this.fruits = items
			}
		}
	},
};
</script>
