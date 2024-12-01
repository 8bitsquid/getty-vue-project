<script setup>
	import { ref, watchEffect } from 'vue';

	const API_URL = "https://data.getty.edu/museum/collection/sparql"
	const DEFAULT_QUERY = `PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
SELECT * WHERE { 
	?sub ?pred ?obj . 
} LIMIT 10`

	// const results = ref([
	// 	{
	// 		sub: "sub1",
	// 		pred: "pred1",
	// 		obj: "obj1"
	// 	},
	// 	{
	// 		sub: "sub2",
	// 		pred: "pred2",
	// 		obj: "obj2"
	// 	},
	// 	{
	// 		sub: "sub3",
	// 		pred: "pred3",
	// 		obj: "obj3"
	// 	}
	// ]);
	

	const queryString = ref(DEFAULT_QUERY)
	const results = ref({})

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
			if (!resp.ok) {
				throw new Error(`Unable to query collections`);
			}

			const json = await resp.json();

			if (json.results && json.results.bindings && json.results.bindings.length > 0){
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

		<div class="row">
			<div class="query-box">
				<form @submit.prevent="runQuery">
					<div class="row">
						<div class="col">
							<textarea class="query-input" v-model="queryString"></textarea>
						</div>
						<div class="col">
							<input type="submit" value="Run Query" class="btn btn-primary" />
						</div>
					</div>
				</form>
			</div>
		</div>

		<table class="table">
			<thead>
				<tr>
					<th scope="col">sub</th>
					<th scope="col">pred</th>
					<th scope="col">obj</th>
				</tr>
			</thead>
			<tbody>
				<tr v-for="item in results">
					<td>{{ item.sub.value }}</td>
					<td>{{ item.pred.value }}</td>
					<td>{{ item.obj.value  }}</td>
				</tr>
			</tbody>
		</table>
	</div>
</template>

<style>
	.query-box {
		textarea {
			width: 100%;
			height: 300px;
		}
	}
</style>