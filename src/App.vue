<template>
	<div>
		<Navbar :appName="appName" />
		<main class="container pt-5">
			<form v-on:submit.prevent="saveTodo">
				<div class="mb-3">
					<label for="exampleInputEmail1" class="form-label fs-4"> Add activity</label>
					<input
						type="text"
						class="form-control"
						id="exampleInputEmail1"
						v-model="inputTodo"
					/>
					<div id="emailHelp" class="form-text text-secondary pt-lg-2">
						<sup class="text-danger fs-6">*</sup>
						Sleep, Learn Vue, Learn React, etc...
					</div>
				</div>

				<div class="mb-3 form-check">
					<input
						type="checkbox"
						class="form-check-input"
						id="exampleCheck1"
						v-model="reminderMe"
					/>
					<label class="form-check-label" for="exampleCheck1">remind me</label>
				</div>
				<button
					type="submit"
					class="btn btn-primary fw-semibold fs-5 col-lg-1"
					:disabled="!inputTodo.length"
				>
					+
				</button>
			</form>

			<section class="pt-5">
				<label class="fs-4">Todo list</label>
				<ol class="list-group list-group-numbered">
					<li
						class="list-group-item d-flex justify-content-between align-items-start"
						v-for="todo in todoList"
						:key="todo.id"
					>
						<div class="ms-2 me-auto">
							<div
								:class="{ 'fw-bold bg-primary text-white px-2 rounded': todo.remindMe }"
							>
								{{ todo.text }}
							</div>
							{{ todo.time }}
						</div>
						<i
							v-on:click="testChange"
							class="bi bi-x-lg bg-danger rounded-circle text-white px-2 py-1"
							role="button"
							@click="deleteTodo(todo.id)"
						></i>
					</li>
				</ol>
			</section>
		</main>
	</div>
</template>

<script>
import { v4 as uuidV4 } from "uuid";
import Navbar from "./components/Navbar.vue";
export default {
	name: "App",
	components: {
		Navbar,
	},
	data() {
		return {
			appName: "Simple Todo Vue",
			inputTodo: "",
			reminderMe: false,
			todoList: [],
		};
	},
	mounted() {
		if (localStorage.getItem("todo"))
			this.todoList = JSON.parse(localStorage.todo ?? "[]");
	},
	methods: {
		saveTodo() {
			const newTodo = {
				id: uuidV4(),
				text: this.inputTodo,
				remindMe: this.reminderMe,
				time: new Date().toLocaleTimeString().slice(0, -6),
			};

			this.todoList = [newTodo, ...this.todoList];
			localStorage.setItem("todo", JSON.stringify(this.todoList));
			this.inputTodo = "";
			this.reminderMe = false;
		},
		deleteTodo(id) {
			const newTodos = this.todoList.filter((todo) => todo.id !== id);
			this.todoList = newTodos;
			localStorage.setItem("todo", JSON.stringify(newTodos));
		},
	},
};
</script>
