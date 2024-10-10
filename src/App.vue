<script setup>
import { ref, computed } from 'vue'

const header = ref('Shopping list app')
const editing = ref(false)
const items = ref([
	{ id: 1, label: 'hats', purchased: true, highPriority: false },
	{ id: 2, label: 'games', purchased: true, highPriority: false },
	{ id: 3, label: 'cups', purchased: false, highPriority: true },
])

const newItem = ref('')
const newItemHighPriority = ref(false)
const iceCreamFlavors = ref([])
const reversedItems = computed(() => [...items.value].reverse())

const saveItem = () => {
	if (newItem.length === 0) return
	items.value.push({
		id: items.value.length + 1,
		label: newItem.value,
		highPriority: newItemHighPriority.value,
	})
	newItem.value = ''
	newItemHighPriority.value = false
}

const doEdit = e => {
	editing.value = e
	newItem.value = ''
	newItemHighPriority.value = false
}

const togglePurchased = item => {
	item.purchased = !item.purchased
}
</script>

<template>
	<div class="header">
		<h1>{{ header }}</h1>
		<button class="btn" v-if="editing" @click="doEdit(false)">Cancel</button>
		<button class="btn btn-primary" v-else @click="doEdit(true)">
			Add Item
		</button>
	</div>

	<form class="add-item-form" v-if="editing" @submit.prevent="saveItem">
		<input v-model.trim="newItem" type="text" placeholder="Add an item" />

		<label>
			<input type="checkbox" v-model="newItemHighPriority" />
			High priority
		</label>

		<button class="btn btn-primary" :disabled="newItem.length === 0">
			Save Item
		</button>
	</form>

	<ul>
		<li
			v-for="item in reversedItems"
			@click="togglePurchased(item)"
			:key="item.id"
			class="static-class"
			:class="{
				strikeout: item.purchased,
				priority: item.highPriority,
			}"
		>
			{{ item.label }}
		</li>
	</ul>

	<p v-if="!items.length">Nothing to see here</p>
</template>
