<script setup>
	import { ref } from 'vue';

	const defaultQuery = `SELECT * WHERE { 
  ?sub ?pred ?obj . 
} LIMIT 10`

	const query = ref(defaultQuery);
</script>

<template>
	 <h2>Query Getty Collections</h2>
	<textarea v-model="query"></textarea>
	<QueryResults 
		:results="results"
	/>
</template>