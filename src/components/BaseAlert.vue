<script setup>
	import { computed, ref, watchEffect } from 'vue';

	const { level, message } = defineProps({
		level: {
			type: String,
			default: "danger"
		},
		message: String,
	})
	const isVisible = ref(false)
	const alertLevel = computed(() => "alert-" + level);
	const visibility = computed(() => isVisible.value ? 'visible' : 'invisible')
	const errorMessage = computed(() => isVisible.value ? message : '')

	watchEffect(() => {
		if (message) {
			isVisible.value = true
		}
	});

	function close() {
		isVisible.value = false
	}
</script>

<template>
	<div class="alert alert-dismissible" :class="[alertLevel, visibility]" role="alert">
		{{ errorMessage }}
		<button type="button" class="btn-close" data-bs-dismiss="alert" aria-label="Close" @click="close"></button>
	</div>
</template>