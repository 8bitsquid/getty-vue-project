<script setup>
	defineProps({
		queryString: String,
		submitHandler: Function
	});
	defineEmits(['fetchQueryResults'])

	const query = defineModel('queryString');
</script>

<template>
	<div class="row">
		<div class="query-box">
			<form @submit.prevent="$emit('fetchQueryResults')">
				<div class="row">
					<div class="col-9">
						<textarea v-model="query" @keydown.shift.enter="$emit('fetchQueryResults')"></textarea>
					</div>
					<div class="col-3">
						<input type="submit" value="Run Query" class="btn btn-primary" />
					</div>
				</div>
			</form>
		</div>
		<small class="text-body-secondary">SHIRT + ENTER to run query</small>
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