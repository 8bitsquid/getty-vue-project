<script setup>
import { defineProps, ref } from 'vue';

defineProps(['text']);

const isChecked = ref(false);

</script>

<template>
	<div :class="{done: isChecked}">
		<input type="checkbox"  v-model="isChecked" />
		{{ text }} {{ isChecked }}
	</div>
</template>

<style>
	.done {
		text-decoration: line-through
	}
</style>