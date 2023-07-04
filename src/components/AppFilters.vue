<template>
	<aside class="app-filters">
		<section class="toggle-group">
			<button v-for="filter in filters" :key="filter" class="button" :class="{ 'button--primary': currentFilter === filter }" @click="changeFilter(filter)">{{ filter }}</button>
		</section>
	</aside>
</template>

<script lang="ts">
	import { defineComponent, PropType } from 'vue';
	import { Filter } from '../types/Filter';

	interface State {
		filters: Filter[];
	}

	export default defineComponent({
		props: {
			currentFilter: {
				type: String as PropType<Filter>,
				required: true,
			},
		},
		data(): State {
			return {
				filters: ['All', 'Active', 'Done'],
			};
		},
		methods: {
			changeFilter(chosenFilter: Filter) {
				this.$emit('changeFilter', chosenFilter);
			},
		},
		emits: {
			changeFilter: (chosenFilter: Filter) => chosenFilter,
		},
	});
</script>
