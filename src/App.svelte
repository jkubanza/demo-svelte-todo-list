<script>
	import DeleteIcon from './svg/DeleteIcon.svelte';
	import EmptyCard from './svg/EmptyCard.svelte';
	import "./css/rest.css";
	import "./css/style.css";

	let todoTitle = "";
	let todoId = 4;
	let currentFilter = "all";

	let todos = [
		{ id: 1, title: "My first todo", isCompleted: false, isEditing: false },
		{
			id: 2,
			title: "My second todo",
			isCompleted: true,
			isEditing: false,
		},
		{ id: 3, title: "My third todo", isCompleted: false, isEditing: false },
	];

	$: remainingTodo = todos.filter((todo) => !todo.isCompleted).length;

	$: filteredTodos =
		currentFilter === "all"
			? todos
			: currentFilter === "active"
			? todos.filter((todo) => !todo.isCompleted)
			: todos.filter((todo) => todo.isCompleted);
	function addTodo() {
		todos = [
			...todos,
			{
				id: todoId,
				title: todoTitle,
				isCompleted: false,
				isEditing: false,
			},
		];
		todoId++;
		todoTitle = "";
	}

	function removeTodo(id) {
		todos = todos.filter((todo) => todo.id !== id);
	}
	function checkAllTodo() {
		todos = todos.map((todo) => {
			todo.isCompleted = true;
			return todo;
		});
	}

	function clearCompletedTodo() {
		todos = todos.filter((todo) => !todo.isCompleted);
	}

	let beforeEditCache = "";
	function editTodo(todo) {
		beforeEditCache = todo.title;
		todo.isEditing = true;
		todos = todos;
	}
	function doneEdit(todo) {
		if (todo.title.trim().length === 0) {
			todo.title = beforeEditCache;
		}
		todo.isEditing = false;
		todos = todos;
	}
	function doneEditKeydown(event, todo) {
		if (event.key === "Enter") {
			doneEdit(todo);
		}
		if (event.key === "Escape") {
			todo.title = beforeEditCache;
			doneEdit(todo);
		}
	}
</script>

<div class="todo-app-container">
	<div class="todo-app">
		<h2>Todo App</h2>
		<form action="#" on:submit|preventDefault={addTodo}>
			<input
				type="text"
				class="todo-input"
				placeholder="What do you need to do?"
				bind:value={todoTitle}
			/>
		</form>
		{#if todos.length > 0}
			<ul class="todo-list">
				{#each filteredTodos as todo}
					<li class="todo-item-container">
						<div class="todo-item">
							{#if !todo.isEditing}
								<input
									id={todo.id}
									type="checkbox"
									bind:checked={todo.isCompleted}
								/>
								<label
									on:dblclick={editTodo(todo)}
									for={todo.id}
									class="todo-item-label"
									class:line-through={todo.isCompleted}
									>{todo.title}</label
								>
							{:else}
								<input
									type="text"
									class="todo-item-input"
									bind:value={todo.title}
									on:blur={doneEdit(todo)}
									on:keydown={(event) =>
										doneEditKeydown(event, todo)}
									autofocus
								/>
							{/if}
						</div>
						<button class="x-button" on:click={removeTodo(todo.id)}>
							<DeleteIcon />
						</button>
					</li>
				{/each}
			</ul>

			<div class="check-all-container">
				<div>
					<button class="button" on:click={checkAllTodo}
						>Check All</button
					>
				</div>

				<span> {remainingTodo} items remaining</span>
			</div>

			<div class="other-buttons-container">
				<div>
					<button
						on:click={() => (currentFilter = "all")}
						class="button filter-button"
						class:filter-button-active={currentFilter === "all"}
					>
						All
					</button>
					<button
						on:click={() => (currentFilter = "active")}
						class="button filter-button"
						class:filter-button-active={currentFilter === "active"}
						>Active
					</button>
					<button
						on:click={() => (currentFilter = "completed")}
						class="button filter-button"
						class:filter-button-active={currentFilter ===
							"completed"}>Completed</button
					>
				</div>
				<div>
					<button on:click={clearCompletedTodo} class="button"
						>Clear completed</button
					>
				</div>
			</div>
		{:else}
			<div class="no-todos-container">
				<div class="no-todos-svg-container">
					<EmptyCard/>
				</div>
				<div class="no-todos-text-container">
					Please enter a todo...
				</div>
			</div>
		{/if}
	</div>
</div>
