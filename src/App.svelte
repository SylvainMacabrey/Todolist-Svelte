<script>
	import { afterUpdate } from 'svelte';

	export let todoItems;
	export let newTodo;

	afterUpdate(() => {
		document.querySelector('.js-todo-input').focus();
	});

	function addTodo() {
		newTodo = newTodo.trim();
  		if (!newTodo) return;
		const todo = {
			text: newTodo,
			checked: false,
			id: Date.now(),
		};
		todoItems = [...todoItems, todo];
		newTodo = '';
	}

	function toggleDone(id) {
		const index = todoItems.findIndex(item => item.id === Number(id));
		todoItems[index].checked = !todoItems[index].checked;
		console.log('bonsoir')
	}

	function deleteTodo(id) {
		todoItems = todoItems.filter(item => item.id !== Number(id));
	}

</script>

<main>
	<h1>Ce que j'ai à faire</h1>
	<form on:submit|preventDefault={ addTodo }>
		<input class="js-todo-input" type="text" aria-label="Enter a new todo item" placeholder="Nouvelle tâche" bind:value={ newTodo }>
	</form>
	<ul class="todo-list">
		{ #each todoItems as todo (todo.id) }
		  <li class="todo-item { todo.checked ? 'done' : '' }">
			<input id={ todo.id } type="checkbox" />
			<label for={ todo.id } class="tick" on:click={ () => toggleDone(todo.id) }></label>
			<span>{ todo.text }</span>
			<button class="delete-todo" on:click={ () => deleteTodo(todo.id) }>
				<svg><use href="#delete-icon"></use></svg>
			</button>
		  </li>
		{/each}
	  </ul>
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: #ff3e00;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>