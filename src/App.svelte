<script>
  import { afterUpdate } from 'svelte';

  let todoItems = [];
  let newTodo = "";

  function addTodo() {
    newTodo = newTodo.trim();
    if (!newTodo) return;

    const todo = {
      text: newTodo,
      checked: false,
      id: Date.now()
    };

    todoItems = [...todoItems, todo];
    newTodo = "";
    console.log(todoItems);
  }
  function toggleDone(id) {
    const index = todoItems.findIndex(item=>item.id === Number(id));
    todoItems[index].checked = !todoItems[index].checked;
  }
  function deleteTodo(id) {
    todoItems = todoItems.filter(item=>item.id!==Number(id));
  }
  afterUpdate(()=>{
    document.querySelector('.js-todo-input').focus();
  });
</script>

<main>
  <div class="container">
    <h1 class="app-title">TODOS</h1>
    <ul class="todo-list">
      {#each todoItems as todo (todo.id)}
        <li class="todo-item {todo.checked ? 'done' : ''}">
          <input id={todo.id} type="checkbox" />
          <label for={todo.id} class="tick" on:click={() => toggleDone(todo.id)}></label>
          <span>{todo.text}</span>
          <button class="delete-todo" on:click={()=>deleteTodo(todo.id)}>
            <svg>
              <use href="#delete-icon"></use>
            </svg>
          </button>
        </li>
        {/each}
    </ul>
    <div class="empty-state">
      <svg class="checklist-icon">
        <use href="#checklist-icon" />
      </svg>
      <h2 class="empty-state__title">Add your first Todo</h2>
      <p class="empty-state__description">What you want to do?</p>
    </div>
    <form on:submit|preventDefault={addTodo}>
      <input
        type="text"
        class="js-todo-input"
        aria-label="Enter a new todo item"
        placeholder="E.g. Build a web app"
        bind:value={newTodo} />
    </form>
  </div>
</main>
