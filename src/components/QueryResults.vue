<script setup>
import { computed, ref, watch } from 'vue';

const { results, stats } = defineProps({
	results: Array,
	querySeconds: Number
});
const currPage = ref(0)
const numPerPage = ref(50);
const totalPages = computed(() => Math.ceil(results.length  / numPerPage.value));
const compactView = ref(false);
const wrapText = ref(false);
const wrapTextClass = computed(() => wrapText.value ? '' : 'text-nowrap');

const filteredResults = computed(() => {
	if (results.length > 0) {
		let perpage = parseInt(numPerPage.value)
		let start = (currPage.value * perpage) + currPage.value;
		let end = start + perpage + 1;
		return results.slice(start, end)
	}
	else {
		return results
	}
});

function setPage(page){
	currPage.value = page - 1;
}

function prevPage(){
	if (currPage.value > 0) {
		setPage(currPage.value - 1);
	}
}

function nextPage() {
	if (currPage.value < totalPages.value) {
		setPage(currPage.value + 1);
	}
}

watch(numPerPage, (newVal, oldVal) => {
	if (newVal !== oldVal) {
		setPage(1)
	} 
});

</script>

<template>
	<div class="row">
		<div class="col-2 offset-6">
			<div class="form-check form-switch">
				<input class="form-check-input" type="checkbox" role="switch" id="compact_view" v-model="compactView" />
				<label class="form-check-label" for="compact_view">Compact View</label>
			</div>
		</div>
		<div class="col-2">
			<div class="form-check form-switch">
				<input class="form-check-input" type="checkbox" role="switch" id="wrap_column_text" v-model="wrapText" />
				<label class="form-check-label" for="wrap_column_text">Wrap Column Text</label>
			</div>
		</div>
		<div class="col-2">
			<div class="form-floating">
				<select class="form-select" aria-label="Number of results per page" id="per_page_select" v-model="numPerPage">
					<option>10</option>
					<option>50</option>
					<option>100</option>
					<option>1000</option>
				</select>
				<label for="per_page_select">Results Per Page</label>
			</div>
		</div>
	</div>

	<div class="row">
		<small>Retrieved {{ results.length }} results in {{ querySeconds }}ms</small>
		<div class="table-responsive">
			<table class="table table-striped table-hover" :class="{'table-sm': compactView}">
				<thead>
					<tr>
						<th scope="col">sub</th>
						<th scope="col">pred</th>
						<th scope="col">obj</th>
					</tr>
				</thead>
				<tbody>
					<tr v-for="(item, index) in filteredResults">
						<td :class="wrapTextClass">{{ item.sub.value }}</td>
						<td :class="wrapTextClass">{{ item.pred.value }}</td>
						<td :class="wrapTextClass">{{ item.obj.value  }}</td>
					</tr>
				</tbody>
			</table>
		</div>
	</div>

	<div class="row" v-if="totalPages > 1">
		<nav aria-label="Query results page navigation">
			<ul class="pagination">
				<li class="page-item">
					<a class="page-link" :class="{disabled: currPage === 0}" href="#" @click="prevPage">Previous</a>
				</li>
				<li class="page-item" v-for="page in totalPages">
					<a class="page-link" :class="{active: page-1 === currPage}" href="#" @click="setPage(page)">{{ page }}</a>
				</li>
				<li class="page-item">
					<a class="page-link" :class="{disabled: currPage+1 == totalPages}" href="#" @click="nextPage">Next</a>
				</li>
			</ul>
		</nav>
	</div>
</template>