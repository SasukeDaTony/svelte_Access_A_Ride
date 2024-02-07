<!-- Javscript -->
<script>
  import { authHandlers, authStore } from "../../store/store";
  import { getDoc, doc, setDoc } from "firebase/firestore";
  import { auth, db } from "../../lib-firebase/firebase";
  import TodoItem from "../../components/TodoItem.svelte";

  let todoList = [];
  let currTodo = "";
  let error = false;

  authStore.subscribe((curr) => {
    todoList = curr.data.todos;
  });

  function addTodo() {
    error = false;
    if (!currTodo) {
      error = true;
      index -= 1;
    }
    todoList = [...todoList, currTodo];
    currTodo = "";
  }

  function editTodo(index) {
    let newTodoList = todoList.filter((val, i) => {
      return i !== index;
    });
    currTodo = todoList[index];
    todoList = newTodoList;
  }

  function removeTodo(index) {
    let newTodoList = todoList.filter((val, i) => {
      return i !== index;
    });
    todoList = newTodoList;
  }

  async function saveTodos() {
    try {
      const userRef = doc(db, "users", $authStore.user.uid);
      await setDoc(
        userRef,
        {
          todos: todoList,
        },
        { merge: true }
      );
    } catch (err) {
      console.log("Error! Failed To Save");
    }
  }
</script>

<!-- HTML -->

{#if !$authStore.loading}
  <div class="mainContainer">
    <div class="headerContainer">
      <h1>Task Manager</h1>
      <div class="headerButtons">
        <button on:click={saveTodos}
          ><i class="fa-regular fa-floppy-disk"></i>Save</button
        >
        <button on:click={authHandlers.logout}
          ><i class="fa-solid fa-door-open"></i>Log Out</button
        >
      </div>
    </div>
    <main>
      {#if todoList.length === 0}
        <p>You Have No Tasks Yet!</p>
      {/if}
      {#each todoList as todo, index}
        {#if todo !== ""}
          <TodoItem {todo} {index} {removeTodo} {editTodo} />
        {/if}
      {/each}
    </main>
    <div class={"enterTodo " + (error ? "errorBorder" : "")}>
      <input
        bind:value={currTodo}
        type="text"
        placeholder={error
          ? "Please Write A Task First"
          : "Write Your Task Here"}
      />
      <button on:click={addTodo}>Add Task</button>
    </div>
  </div>
{/if}

<!-- CSS -->

<style>
  .mainContainer {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    gap: 24px;
    padding: 24px;
    width: 100%;
    max-width: 1000px;
    margin: 0 auto;
  }

  .headerContainer {
    display: flex;
    align-items: center;
    justify-content: space-between;
  }
  .headerContainer h1 {
    border-bottom: 1px solid white;
  }
  .headerButtons {
    display: flex;
    gap: 14px;
    justify-content: flex-start;
  }

  .headerContainer button {
    background: #003c5b;
    color: white;
    padding: 10px 10px;
    border: none;
    border-radius: 4px;
    font-weight: 700;
    display: flex;
    align-items: center;
    gap: 8px;
    cursor: pointer;
  }

  .headerContainer button i {
    font-size: 1.1rem;
  }

  .headerContainer button:hover {
    opacity: 0.7;
  }

  main {
    display: flex;
    flex-direction: column;
    gap: 8px;
    flex: 1;
  }

  .enterTodo {
    display: flex;
    align-items: stretch;
    border: 1px solid #0891b2;
    border-radius: 5px;
    overflow: hidden;
  }

  .errorBorder {
    border-color: coral !important;
  }

  .enterTodo input {
    background: transparent;
    border: none;
    padding: 14px;
    color: white;
    flex: 1;
  }

  .enterTodo input:focus {
    outline: none;
  }

  .enterTodo button {
    padding: 0 28px;
    background: #003c5b;
    border: none;
    color: cyan;
    font-weight: 600;
    cursor: pointer;
  }

  .enterTodo button:hover {
    background: transparent;
  }
</style>
