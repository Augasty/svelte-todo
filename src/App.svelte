<script>
  import { afterUpdate } from "svelte";

  let todoItems = [];

  // Function to load data from localStorage
  function loadDataFromLocalStorage() {
    const storedData = localStorage.getItem("sveltetodo");
    if (storedData) {
      todoItems = JSON.parse(storedData);
    }
  }

  // Call the load data function on component initialization
  loadDataFromLocalStorage();

  // Define a variable to store the latest version of todoItems
  let latestTodoItems = [];

  //$:: This is a Svelte reactive statement. It's a way to declare reactive behavior in a Svelte component. When any of the variables or expressions inside the statement change, the code block following $: is executed.
  $: {
    latestTodoItems = todoItems;
    afterUpdate(() => {
      localStorage.setItem("sveltetodo", JSON.stringify(latestTodoItems));
	  document.querySelector('.js-todo-input').focus();
    });
  }

  // Function to add an item to the todo list
  function addItem(item) {
    todoItems = [...todoItems, item];
  }
  let newTodo = "";

  function addTodo() {
    newTodo = newTodo.trim(); //to remove the whitespace before and after the string
    if (!newTodo) return;

    const todo = {
      text: newTodo,
      checked: false,
      id: Date.now(),
    };

    todoItems = [...todoItems, todo];

    //   storing the list in localstorage, not required anymore because we are using the svelte reactive statement
    // localStorage.setItem("sveltetodo", JSON.stringify(todoItems));
    // console.log(todoItems);

    newTodo = "";
  }

  function toggleDone(id) {
    const index = todoItems.findIndex((item) => item.id === Number(id));
    todoItems[index].checked = !todoItems[index].checked;
  }

  function deleteTodo(id) {
    todoItems = todoItems.filter((item) => item.id !== Number(id));
    console.log(todoItems);
  }
</script>

<main>
  <div class="container">
    <h1 class="app-title">todos</h1>
    <ul class="todo-list">
      {#each todoItems as todo (todo.id)}
        <li class="todo-item {todo.checked ? 'done' : ''}">
          <input id={todo.id} type="checkbox" />
          <!-- svelte-ignore a11y-click-events-have-key-events -->
          <label
            for={todo.id}
            class="tick"
            on:click={() => toggleDone(todo.id)}
          />
          <span>{todo.text}</span>
          <button class="delete-todo" on:click={() => deleteTodo(todo.id)}>
            <svg><use href="#delete-icon" /></svg>
          </button>
        </li>
      {/each}
    </ul>
    <div class="empty-state">
      <svg class="checklist-icon"><use href="#checklist-icon" /></svg>
      <h2 class="empty-state__title">Add your first todo</h2>
      <p class="empty-state__description">
        What do you want to get done today?
      </p>
    </div>
    <form on:submit|preventDefault={addTodo}>
      <input
        class="js-todo-input"
        type="text"
        aria-label="Enter a new todo item"
        placeholder="E.g. Build a web app"
        bind:value={newTodo}
      />
    </form>
  </div>
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
