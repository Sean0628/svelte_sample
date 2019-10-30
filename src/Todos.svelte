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
</script>
