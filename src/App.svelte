<script>
	import {Toast} from "bootstrap/dist/js/bootstrap.bundle.js";

	let todos = [];
	let todo = { id: "", text: "", state: false };

	let toastEl;
	let toastOptions = {text: "", color: ""};
	let toastInstance;

	if (localStorage.getItem('todos')) {
		todos = JSON.parse(localStorage.getItem('todos'));
	}

	const showToast = (text, color) => {
		toastOptions = {text, color};
		toastInstance.show();
	};

	const addTodos = () => {
		if (!todo.text.trim()) {
			todo.text = "";
			return;
		}
		todo.id = new Date().getTime().toString();
		todos = [...todos, todo];
		todo = { id: "", text: "", state: false };
		showToast("New todo added", "success");
	};

	const delTodo = (id) => {
		todos = todos.filter(todo => todo.id !== id);
		showToast("Todo removed", "danger");
	};

	const editTodo = (id) => {
		todos = todos.map(todo => todo.id === id ? {...todo, state: !todo.state} : todo);
		console.log(todos);
	}

	$: localStorage.setItem("todos", JSON.stringify(todos));

	$: if (toastEl) {
		toastInstance = new Toast(toastEl);
	}

	const buttonClass = (valor) => valor ? "btn btn-sm btn-success" : "btn btn-sm btn-warning";
	const iconClass = (valor) => (valor ? "bi bi-arrow-clockwise" : "bi bi-check2");
</script>

<div class="container">
	<h1 class="display-5">CRUD</h1>
	<p class="my-3 display-6">A basic Todo App made using Svelte & Bootstrap Toast</p>
	
	<form on:submit|preventDefault="{addTodos}">
		<input
			type="text"
			class="form-control"
			bind:value="{todo.text}"
			placeholder="Enter para agregar tarea"
		/>
	</form>

	{#each todos as todo}
		<div class="shadow my-3 p-3">
			<p class={todo.state ? "text-decoration-line-through" : ""}>{todo.text}</p>
			<button type="button" class={buttonClass(todo.state)} on:click={editTodo(todo.id)}>
				<i class={iconClass(todo.state)}></i>
			</button>
			<button type="button" class="btn btn-sm btn-danger" on:click={delTodo(todo.id)}><i class="bi bi-trash"></i></button>
		</div>
	{/each}
	
	<div class="toast-container position-absolute top-0 end-0 p-3">
		<div bind:this="{toastEl}" class="toast align-items-center text-white bg-{toastOptions.color} border-0"
			role="alert"
			aria-live="assertive"
			aria-atomic="true">
			<div class="d-flex">
				<div class="toast-body">{toastOptions.text}</div>
				<button
					type="button"
					class="btn-close btn-close-white me-2 m-auto"
					data-bs-dismiss="toast"
					aria-label="Close">
				</button>
			</div>
		</div>
	</div>

</div>

<style>
</style>