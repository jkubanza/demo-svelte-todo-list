<script>
	import "./css/rest.css";
	import "./css/style.css";

	let todoTitle = "";
	let todoId = 4;

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
		todos=todos.map((todo)=>{
			todo.isCompleted=true;
			return todo;
		})
	}

	function clearCompletedTodo() {
		todos=todos.filter(todo=>!todo.isCompleted)
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

		<ul class="todo-list">
			{#each todos as todo}
				<li class="todo-item-container">
					<div class="todo-item">
						<input
							id={todo.id}
							type="checkbox"
							bind:checked={todo.isCompleted}
						/>
						<label
							for={todo.id}
							class="todo-item-label"
							class:line-through={todo.isCompleted}
							>{todo.title}</label
						>
					</div>
					<button class="x-button" on:click={removeTodo(todo.id)}>
						<svg
							class="x-button-icon"
							fill="none"
							viewBox="0 0 24 24"
							stroke="currentColor"
						>
							<path
								strokeLinecap="round"
								strokeLinejoin="round"
								strokeWidth={2}
								d="M6 18L18 6M6 6l12 12"
							/>
						</svg>
					</button>
				</li>
			{/each}
		</ul>

		<div class="check-all-container">
			<div>
				<button class="button" on:click={checkAllTodo}>Check All</button>
			</div>

			<span> {remainingTodo} items remaining</span>
		</div>

		<div class="other-buttons-container">
			<div>
				<button class="button filter-button filter-button-active">
					All
				</button>
				<button class="button filter-button">Active</button>
				<button class="button filter-button">Completed</button>
			</div>
			<div>
				<button on:click={clearCompletedTodo} class="button">Clear completed</button>
			</div>
		</div>
	</div>
</div>
