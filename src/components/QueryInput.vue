<script setup>
	defineProps({
		queryString: String,
		submitHandler: Function
	});

	const query = defineModel('queryString');
</script>

<template>
	<div class="query-box">
		<form @submit.prevent="$emit('fetchQueryResults')">
			<div class="row">
				<div class="col-9">
					<textarea v-model="query"></textarea>
				</div>
				<div class="col-3">
					<input type="submit" value="Run Query" class="btn btn-primary" />
				</div>
			</div>
		</form>
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