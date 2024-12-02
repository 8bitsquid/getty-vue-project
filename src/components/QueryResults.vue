<script setup>
defineProps(['results']);
</script>

<template>
	<table class="table table-striped">
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
</template>