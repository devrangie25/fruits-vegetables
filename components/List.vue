<template>
	<div>
		<div class="title">{{ listTitle }}</div>
		<div :class="`${title}-container-list`">
			<div :class="`${title}-list`">
				<ul :class="`${title === 'fruits' ? 'fu' : 've'}-list`">
					<draggable
						v-model="itemsCloned"
						group="people"
						@start="drag = true"
						handle=".handle"
						:move="checkMove"
						@change="log"
						@end="drag = true"
					>
						<li
							v-for="(item, i) in itemsCloned"
							:key="i"
							:id="generateId(item.name, i)"
							:class="`${
								title === 'fruits' ? 'fu' : 've'
							}-list-item`"
							@mouseover="hover = i"
							@mouseleave="hover = null"
						>
							<div style="display: flex">
								<img
									v-show="hover === i"
									src="@/assets/img/direction.png"
									width="20"
									class="drag-img handle"
								/>
								<span style="margin-left: 0.2rem">
									{{ item.name }}
								</span>
							</div>
						</li>
					</draggable>
				</ul>
			</div>
		</div>
	</div>
</template>

<script>
import draggable from "vuedraggable";

export default {
	name: "list",
	components: {
		draggable,
	},
	props: {
		title: {
			default: "Default Title",
			required: false,
			type: String,
		},
		items: Array,
		group: String,
	},

	data() {
		return {
			hover: null,
			itemsCloned: [...this.items],
			isFinishDrag: false
		};
	},

	watch: {
		itemsCloned: function (newItems) {
			let self = this;
			this.$emit("new-sorted-items", newItems, self.listTitle);
		},
	},

	computed: {
		listTitle() {
			return `${this.title.charAt(0).toUpperCase()}${this.title.substr(1).toLowerCase()}`;
		},
	},

	methods: {

		log: function (evt) {
			console.log("log", evt);
		},

		checkMove(evt) {

			const { dragged, related } = evt

			if (dragged._prevClass !== related._prevClass) {
				this.$swal.fire({
					position: 'center',
					icon: 'warning',
					title: 'Permission Denied',
					text: `${evt.draggedContext.element.name} is not a ${dragged._prevClass === 'fu-list-item' ? 'Vegetable' : 'Fruit'}.`,
					showConfirmButton: false,
					timer: 1500
				})
				return false
			}

			return true
		},

		generateId(name, ind) {
			return `${name.toLowerCase()}-${ind}`;
		},
	},
};
</script>
