<script>
  export let index;
  export let todo;
  export let editTodo;
  export let removeTodo;
</script>

<!-- HTML -->
<div class="todo">
  <p>
    {index + 1}. {todo}
  </p>
  <div class="actions">
    <i
      role="button"
      tabindex="0"
      class="fa-regular fa-pen-to-square"
      on:click={() => {
        editTodo(index);
      }}
      on:keydown={() => {}}><span class="square">Edit</span></i
    >
    <i
      role="button"
      tabindex="0"
      class="fa-regular fa-trash-can"
      on:click={() => {
        removeTodo(index);
      }}
      on:keydown={() => {}}><span class="square">Delete</span></i
    >
  </div>
</div>

<!-- CSS -->
<style>
  .todo {
    border-left: 1px solid cyan;
    padding: 4px 8px;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .actions {
    display: flex;
    align-items: center;
    gap: 14px;
  }

  .fa-pen-to-square:hover {
    color: #d3c722;
    cursor: pointer;
  }

  .square {
    transition-duration: 0ms;
    padding: 5px;
    font-family: "Times New Roman", Times, serif;
  }

  .fa-trash-can:hover {
    color: rgba(226, 28, 28, 0.934);
    cursor: pointer;
  }
</style>
