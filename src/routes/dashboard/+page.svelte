<!-- Javscript -->
<script>
    let todoList = ['Buy Groceries']
    let currTodo = '';
    let error = false;


    function addTodo() {
        error = false;
        if(!currTodo) {
            error = true;
            index-=1
        }
        todoList=[...todoList, currTodo]
        currTodo = '';
    }

function editTodo(index) {
    let newTodoList = todoList.filter((val, i) => {
        return i !==index;
    })
    currTodo = todoList[index];
    todoList = newTodoList;
}

function removeTodo(index) {
    let newTodoList = todoList.filter((val, i) => {
        return i !==index;
    })
    todoList = newTodoList;
}

</script>



<!-- HTML -->

<div class="mainContainer">
    <div class="headerContainer">
        <h1>Task Manager</h1>
        <button><i class="fa-regular fa-floppy-disk"></i>Save</button>
    </div>
    <main>
        {#if todoList.length === 0} 
        <p>You Have No Tasks Yet!</p>
        {/if}
        {#each todoList as todo, index}
            {#if todo !== ''}
            <div class="todo">
                <p>
                    {index + 1}. {todo}
                </p>
                <div class="actions">
                    <i role="button" tabindex="0" class="fa-regular fa-pen-to-square" on:click={() => {editTodo(index)}} on:keydown={() => {}}><span class="square">Edit</span></i>
                    <i role="button" tabindex="0" class="fa-regular fa-trash-can" on:click={() => {removeTodo(index)}} on:keydown={() => {}}><span class="square">Delete</span></i>
                </div>
            </div>
            {/if}
          
        {/each}
    </main>
    <div class={"enterTodo " + (error ? "errorBorder" : '')}>
        <input bind:value={currTodo} type="text" placeholder={error ? "Please Write A Task First" : "Write Your Task Here"}/>
        <button on:click={addTodo}>Add Task</button>
    </div>
</div>












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
        display:flex;
        align-items: center;
        justify-content: space-between;
    }
    .headerContainer h1 {
        border-bottom: 1px solid white;
    }

    .headerContainer button {
        background:#003c5b;
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

    .fa-pen-to-square:hover  {
        color:#0891b2;
        cursor:pointer;
    }

    .square {
        transition-duration: 0ms;
        padding: 5px;
        font-family: 'Times New Roman', Times, serif;
    }

    .fa-trash-can:hover {
        color:rgba(255, 0, 0, 0.609);
        cursor:pointer;
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
        border:none;
        padding:14px;
        color: white;
        flex:1;
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
        cursor:pointer;
    }

    .enterTodo button:hover {
        background: transparent;
    }
</style>