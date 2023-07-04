import { defineComponent } from 'vue'; import component from './shims-vue';
<template>
	<div id="app">
		<AppHeader />
		<AppFilters :current-filter="currentFilter" @change-filter="changeFilter" />
		<main class="app-main">
			<AppTodoList :todos="filteredTodos" @remove-todo="removeTodo" @toggle-todo="toggleTodo" />
			<AppAddTodo @add-todo="addTodo" />
		</main>
		<AppFooter :stats="stats" />
	</div>
</template>

<script lang="ts">
	import { defineComponent } from 'vue';
	import AppHeader from './components/AppHeader.vue';
	import AppFilters from './components/AppFilters.vue';
	import AppTodoList from './components/AppTodoList.vue';
	import AppAddTodo from './components/AppAddTodo.vue';
	import AppFooter, { Stats } from './components/AppFooter.vue';
	import { Todo } from './types/Todo';
	import { Filter } from './types/Filter';

	interface State {
		todos: Todo[];
		currentFilter: Filter;
	}

	export default defineComponent({
		components: {
			AppHeader,
			AppFilters,
			AppTodoList,
			AppAddTodo,
			AppFooter,
		},
		data(): State {
			return {
				todos: [
					{ id: 0, text: 'gg', completed: true },
					{ id: 1, text: 'ww', completed: false },
					{ id: 2, text: 'gf', completed: true },
				],
				currentFilter: 'All',
			};
		},
		computed: {
			filteredTodos(): Todo[] {
				switch (this.currentFilter) {
					case 'Active': {
						return this.activeTodos;
					}
					case 'Done': {
						return this.doneTodos;
					}
					case 'All':
					default: {
						return this.todos;
					}
				}
			},
			stats(): Stats {
				return {
					active: this.activeTodos.length,
					done: this.doneTodos.length,
				};
			},
			activeTodos(): Todo[] {
				return this.todos.filter((todo) => !todo.completed);
			},
			doneTodos(): Todo[] {
				return this.todos.filter((todo) => todo.completed);
			},
		},
		methods: {
			addTodo(todo: Todo) {
				this.todos.push(todo);
			},
			toggleTodo(id: number) {
				const targetTodo = this.todos.find((todo: Todo) => todo.id === id);
				if (targetTodo) {
					targetTodo.completed = !targetTodo?.completed;
				}
			},
			removeTodo(id: number) {
				this.todos = this.todos.filter((todo: Todo) => todo.id !== id);
			},
			changeFilter(filter: Filter) {
				this.currentFilter = filter;
			},
		},
	});
</script>
