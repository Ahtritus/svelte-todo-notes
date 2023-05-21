<script>
  import { createEventDispatcher } from "svelte";

  let notes = "";
  let todos = [];
  let showModal = false;
  let newTodoTitle = "";
  let newTodoDescription = "";

  const dispatch = createEventDispatcher();

  function openModal() {
    showModal = true;
  }

  function closeModal() {
    showModal = false;
    resetNewTodoForm();
  }

  function resetNewTodoForm() {
    newTodoTitle = "";
    newTodoDescription = "";
  }

  function createTodo() {
    if (newTodoTitle.trim() !== "") {
      todos = [
        ...todos,
        {
          title: newTodoTitle,
          description: newTodoDescription,
          completed: false,
        },
      ];
      closeModal();
    }
  }

  function removeTodo(index) {
    todos = todos.filter((_, i) => i !== index);
  }

  function toggleCompleted(index) {
    todos[index].completed = !todos[index].completed;
  }
</script>

<div class="container">
  <div class="left-column">
    <textarea bind:value={notes} placeholder="Take notes here" />
  </div>
  <div class="right-column">
    <h2>Todos</h2>
    <button on:click={openModal}>Add Todo</button>
    {#each todos as todo, index}
      <div
        class="todo-card {todo.completed ? 'completed' : ''}"
        on:click={() => toggleCompleted(index)}
      >
        <div>{todo.title}</div>
        <button on:click={() => removeTodo(index)}>Delete</button>
      </div>
    {/each}
  </div>
</div>

{#if showModal}
  <div class="modal" on:click={closeModal}>
    <div class="modal-content" on:click={(e) => e.stopPropagation()}>
      <h3>Create Todo</h3>
      <input type="text" placeholder="Title" bind:value={newTodoTitle} />
      <textarea placeholder="Description" bind:value={newTodoDescription} />
      <div class="modal-actions">
        <button on:click={createTodo}>Create</button>
        <button on:click={closeModal}>Cancel</button>
      </div>
    </div>
  </div>
{/if}

<style>
  .container {
    display: flex;
    height: 100vh;
  }

  .left-column {
    flex: 30%;
    padding: 20px;
    background-color: #f0f0f0;
  }

  .right-column {
    flex: 70%;
    padding: 20px;
    background-color: #ffffff;
  }

  .todo-card {
    margin-bottom: 10px;
    padding: 10px;
    border: 1px solid #ccc;
    background-color: #fff;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .todo-card.completed {
    background-color: #f0f0f0;
    text-decoration: line-through;
  }

  .todo-card button {
    margin-left: 10px;
  }

  .modal {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background-color: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
  }

  .modal-content {
    width: 300px;
    padding: 20px;
    background-color: #fff;
    border-radius: 4px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }

  .modal input,
  .modal textarea {
    width: 100%;
    padding: 8px;
    margin-bottom: 10px;
    border: 1px solid #ccc;
    border-radius: 4px;
    box-sizing: border-box;
  }

  .modal-actions {
    display: flex;
    justify-content: flex-end;
    margin-top: 10px;
  }

  .modal-actions button {
    margin-left: 10px;
  }
</style>
