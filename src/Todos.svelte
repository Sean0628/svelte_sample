<style>
    .container {
        max-width: 800px;
        margin: 10px auto;
    }
    .logo {
        display: block;
        margin: 20px auto;
        width: 50%;
    }
    .todo-input {
        width: 100%;
        padding: 10px, 20px;
        font-size: 18px;
        margin-bottom: 20px;
    }
    .inner-container {
        display: flex;
        align-items: center;
        justify-content: space-between;
        font-size: 16px;
        border-top: 1px solid lightgrey;
        padding-top: 15px;
        margin-bottom: 13px;
    }
    .inner-container-input {
        margin-right: 12px;
    }
    button {
        font-size: 14px;
        background-color: white;
        appearance: none;
    }
    button:hover {
        background: lightseagreen;
    }
    button:focus {
        outline: none;
    }
    .active {
        background: lightseagreen;
    }
</style>

<script>
  import TodoItem from './TodoItem.svelte';

  let newTodoTitle = '';
  let currentFilter = 'all';
  let nextId = 4;

  let todos = [
    {
      id: 1,
      title: 'First todo',
      completed: false
    },
    {
      id: 2,
      title: 'Second todo',
      completed: false
    },
    {
      id: 3,
      title: 'Third todo',
      completed: false
    }
  ];

  function addTodo(event) {
    if (event.key === 'Enter') {
      todos = [...todos, {
        id: nextId,
        title: newTodoTitle,
        completed: false
      }];

      nextId = nextId + 1;
      newTodoTitle = '';
    }
  }

  function checkAllTodos(event) {
    todos.forEach(todo => todo.completed = event.target.checked);
    todos = todos;
  }

  function updateFilter(newFilter) {
    currentFilter = newFilter;
  }

  function clearCompleted() {
    todos = todos.filter(todo => !todo.completed);
  }

  function handleDeleteTodo(event) {
    todos = todos.filter(todo => todo.id !== event.detail.id);
  }

  function handleToggleComplete(event) {
    const todoIndex = todos.findIndex(todo => todo.id === event.detail.id);
    const updatedTodo = { ...todos[todoIndex], completed: !todos[todoIndex].completed};
    todos = [
      ...todos.slice(0, todoIndex),
      updatedTodo,
      ...todos.slice(todoIndex + 1),
    ];
  }


  $: todosRemaining = filteredTodos.filter(todo =>
    !todos.completed).length;
  $: filteredTodos = currentFilter === 'all' ? todos : currentFilter
    === 'completed' ? todos.filter(todo => todo.completed) : todos.filter(todo => !todo.completed)
</script>

<div class='container'>
  <h1 class='logo'>TODO sample</h1>

  <h2>Svelte Todo App</h2>
  <input type="text" class='todo-input' placeholder='Insert your todo here' bind:value={newTodoTitle} on:keydown={addTodo}>

  {#each filteredTodos as todo}
    <div class='todo-item'>
      <TodoItem
        {...todo}
        on:deleteTodo={handleDeleteTodo}
        on:toggleComplete={handleToggleComplete}
      />
    </div>
  {/each}

  <div class="inner-container">
    <div>
      <label>
        <input class="inner-container-input" type='checkbox' on:change={checkAllTodos}>
        Check All
        <div>{todosRemaining} items left</div>
      </label>
    </div>
  </div>

  <div class='inner-container'>
    <div>
      <button on:click={() => updateFilter('all')} class:active="{currentFilter === 'all'}">All</button>
      <button on:click={() => updateFilter('active')} class:active="{currentFilter === 'active'}">Active</button>
      <button on:click={() => updateFilter('completed')} class:active="{currentFilter === 'completed'}">Completed</button>
    </div>
    <div>
      <button on:click={clearCompleted}>ClearCompleted</button>
    </div>
  </div>
</div>
