<script>
    export let checked;
    export let id;
    export let text;
    export let toggleDone;
    export let deleteTodo
</script>



<li class="todo-item {checked ? 'done' : ''}">
    <input id={id} type="checkbox" />
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <label
      for={id}
      class="tick"
      on:click={() => toggleDone(id)}
    />
    <span>{text}</span>
    <button class="delete-todo" on:click={() => deleteTodo(id)}>
      <svg><use href="#delete-icon" /></svg>
    </button>
  </li>