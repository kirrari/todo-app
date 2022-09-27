<script setup>
import { ref, onMounted, computed, watch } from 'vue';

const todos = ref([]);
const name = ref('');

const inputContent = ref('');
const inputCategory = ref('Other');

const sortedTodos = computed(() =>
	todos.value.sort((a, b) => {
		return a.createdAt - b.createdAt;
	})
);

function capitalize(word) {
	if (word === '') return word;
	return word[0].toUpperCase() + word.slice(1);
}

const addTodo = () => {
	todos.value.push({
		content: capitalize(inputContent.value),
		category: capitalize(inputCategory.value),
		done: false,
		createdAt: new Date().getTime(),
		id: new Date().getTime(),
	});
	inputContent.value = '';
	inputCategory.value = 'Other';
};

const removeTodo = (todo) => {
	todos.value = todos.value.filter((td) => td !== todo);
};

watch(name, (newVal) => {
	localStorage.setItem('name', newVal);
});

watch(
	todos,
	(newVal) => {
		localStorage.setItem('todos', JSON.stringify(newVal));
	},
	{ deep: true }
);

onMounted(() => {
	name.value = localStorage.getItem('name') || '';
	todos.value = JSON.parse(localStorage.getItem('todos')) || [];
});
</script>

<template>
	<main class="app">
		<section class="greeting">
			<h2 class="title">
				<span>Welcome back, </span>
				<input
					class="name-input"
					type="text"
					placeholder="Name here"
					v-model="name"
				/>
			</h2>
		</section>

		<section class="create-todo">
			<form @submit.prevent="addTodo">
				<div>
					<h4>What's on your todo list?</h4>
					<input
						type="text"
						placeholder="Make a sandwich, etc..."
						v-model.trim="inputContent"
					/>
				</div>
				<div>
					<h4>Category</h4>
					<div class="options">
						<label>
							<input
								type="radio"
								name="category"
								value="business"
								v-model="inputCategory"
							/>
							<span>Business</span>
						</label>
						<label>
							<input
								type="radio"
								name="category"
								value="personal"
								v-model="inputCategory"
							/>
							<span>Personal</span>
						</label>
					</div>
				</div>
				<app-button type="submit">Add todo</app-button>
			</form>
		</section>

		<section class="todos-list">
			<h3>Your tasks</h3>
			<div class="list">
				<transition-group name="todos-list">
					<div
						v-for="todo in sortedTodos"
						:key="todo.id"
					>
						<div class="todo">
							<p class="category">{{ todo.category }}</p>
							<p class="todo-content">{{ todo.content }}</p>
							<app-button @click="removeTodo(todo)">
								Remove
							</app-button>
						</div>
					</div>
				</transition-group>
			</div>
		</section>
	</main>
</template>

<style scoped>
.app {
	margin: 2% auto;
	display: flex;
	flex-direction: column;
	padding: 15px;
	max-width: 1200px;
}

section {
	margin-bottom: 20px;
}

.title,
.greeting input {
	font-size: 32px;
}

.create-todo h3 {
	font-size: 28px;
}
.name-input {
	outline: none;
	background-color: rgba(0, 0, 0, 0);
	border: none;
	font-size: 24px;
	color: var(--white);
}

.create-todo input {
	color: var(--bg-color);
}
.todo {
	display: flex;
	justify-content: space-between;
	align-items: center;
	padding: 15px;
	border-radius: 12px;
	margin: 10px 0;
	background-color: rgba(24, 24, 24, 0.8);
	column-gap: 70px;
}

.create-todo form {
	display: flex;
	flex-direction: column;
	background-color: rgba(24, 24, 24, 0.8);
	padding: 15px;
	border-radius: 12px;
	margin-top: 2%;
	row-gap: 20px;
	font-size: 20px;
}

.todos-list h3 {
	text-align: center;
}
.todos-list-item {
	display: inline-block;
	margin-right: 10px;
}

.todos-list-enter-active,
.todos-list-leave-active {
	transition: all 0.5s ease;
}

.todos-list-enter-from,
.todos-list-leave-to {
	opacity: 0;
}

.todos-list-move {
	transition: transform 0.5s ease;
}

@media (max-width: 1200px) {
	.app {
		max-width: 970px;
	}
}
@media (max-width: 992px) {
	.app {
		max-width: 750px;
	}
}
@media (max-width: 767px) {
	.app {
		max-width: none;
	}
}
</style>
