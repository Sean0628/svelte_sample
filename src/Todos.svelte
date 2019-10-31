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

  <div class='inner-container'>
    <div>
      <button on:click={() => updateFilter('all')} class:active="{currentFilter === 'all'}">All</button>
      <button on:click={() => updateFilter('active')} class:active="{currentFilter === 'active'}">Active</button>
      <button on:click={() => updateFilter('completed')} class:active="{currentFilter === 'completed'}">Completed</button>
    </div>
    <div>
      <button on:click-{clearCompleted}>ClearCompleted</button>
    </div>
  </div>
</div>
