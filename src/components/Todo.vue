<script setup>
	import { ref } from 'vue';
	import ToDoItem from './ToDoItem.vue';

	const list = ref(["first", "second"," and now there's a third ToDo"]);
</script>

<template>
	<h2>To Do's</h2>

	<ToDoItem 
		v-for="(item, index) in list"
		:item="item"
		:index="index"
		:text="item" 
		:key="index"
	/>

</template>
