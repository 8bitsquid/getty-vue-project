<script setup>
	import { provide, ref } from 'vue';
	
	import QueryResults from './QueryResults.vue';
	import QueryInput from './QueryInput.vue';
	import Alert from './BaseAlert.vue';

	const API_URL = "https://data.getty.edu/museum/collection/sparql"
	const DEFAULT_QUERY = `PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT * WHERE { 
	?sub ?pred ?obj . 
} LIMIT 60`
	

	const queryString = ref(DEFAULT_QUERY)
	const results = ref({})
	const errorMsg = ref('')

	async function runQuery() {
		const headers = new Headers();
		headers.append("Content-Type", "application/x-www-form-urlencoded")
		headers.append("Accept", "application/sparql-results+json")


		const req = new Request(API_URL, {
			method: "POST",
			body: "query=" + encodeURIComponent(queryString.value),
			headers: headers
		});


		try {
			const resp = await fetch(req);
			const json = await resp.json();
			if (!resp.ok) {
				if (json.errors?.length > 0) {
					 errorMsg.value = json.errors[0].detail
				}
				results.value = {};
				throw new Error(`Unable to query collections`);
			}

			if (json.results?.bindings?.length > 0){
				results.value = json.results.bindings
			}
		} catch (err) {
			console.error(err.message)
		}
	}
	
</script>

<template>
	<div class="container">
		<h2>Query Getty Collections</h2>

		<Alert level="danger" :message="errorMsg" />
		<QueryInput v-model:query-string="queryString" @fetch-query-results="runQuery"/>
		<QueryResults :results="results"/>

	</div>
</template>

