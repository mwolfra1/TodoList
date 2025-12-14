<script>
    import {onMount} from 'svelte';

    let todoItem = $state('');
    let todoList = $state([]);
    
    onMount(() => {
        let storedList = localStorage.getItem("storedList");
        if (storedList) {
            todoList = (JSON.parse(storedList))
        }
    })

    function updateList() {
        return localStorage.setItem("storedList", JSON.stringify(todoList));
    }

    // add todoItem to todoList
    function addItem(event) {
        event.preventDefault();
        if (todoItem.trim().length === 0) {
            return;
        }
        todoList = [...todoList, {
            text: todoItem,
            done: false
        }];
        updateList();
        todoItem = '';
    }

    //remove todoItem from todoList
    function removeItem(index) {
        todoList.splice(index,1);
        updateList();
    }

    function clearAll() {
        todoList = '';
        localStorage.clear();
    }
</script>

    <form onsubmit={addItem}>
        <input type="text" bind:value={todoItem}>
        <button type="submit">Add</button>
    </form>

    <div class="todo-list">
        <ul>
            {#each todoList as item, index}
                <li>
                    <input type="checkbox" bind:checked={item.done} onchange={updateList}>
                    <span class:done={item.done}>{item.text}</span>
                    <button type="button" onclick={() => removeItem(index)}>&times;</button>
                </li>
            {/each}
        </ul>
    </div>
    <div class="clear">
        {#if todoList.length > 0}
        <button type="clear" onclick={clearAll}>Clear All</button>
        {:else}
        <button type="clear" disabled>Clear All</button>
        {/if}
    </div>

<style>
    :global(body) {
    background-image: url("/images/todolistbackground.jpg");
    background-repeat: no-repeat;
    background-size: cover;
    }
    .clear {
        display: flex;
        justify-content: center;
        align-items: center;
        font-family: "Carrois Gothic";
    }
    form {
        display: flex;
        justify-content: center;
        align-items: center;
        font-family: "Carrois Gothic";
    }
    ul {
        display: grid;
        align-items: center;
        list-style: none;
    }
    input[type="checkbox"] {
        appearance: none;
        position: relative;
        width: 30px;
        height: 30px;
        background: #e28dbf;
        border-radius: 50%;
        cursor: pointer;
        margin: 1em;
        transform: translateY(1.6em);
    }
    input[type="checkbox"]:before {
        content: '';
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        border-radius: 50%;
    }
    input[type="checkbox"]:checked:after {
        content: '';
        position: absolute;
        left: 9px;
        top: 4px;
        width: 8px;
        height: 14px;
        border: solid #fff;
        border-width: 0 3px 3px 0;
        transform: rotate(45deg);
    }
    input[type="text"] {
    border: 2px solid #e28dbf;
    border-radius: 15px;
    font-family: "Carrois Gothic";
    font-size: 1rem;    
    }
    button[type="submit"] {
        background-color: #e28dbf;
        border: none;
        border-radius: 15px;
        color: white;
        padding: 3px 15px;
        text-decoration: none;
        margin: 1px 10px;
        cursor: pointer;
        font-family: "Carrois Gothic";
        font-size: 1.2rem;
    }
    button[type="button"] {
        background-color: transparent; 
        border: none;
        color: #e28dbf;
        font-family: "Carrois Gothic";
        font-size: 2rem;
        margin: 0.3em;
        transform: translateY(0.14em);
    }
    button[type="clear"] {
        background-color: #e28dbf;
        border: none;
        border-radius: 15px;
        color: white;
        padding: 3px 15px;
        text-decoration: none;
        margin: 1px 10px;
        cursor: pointer;
        font-family: "Carrois Gothic";
        font-size: 1.2rem;
    }
    li span.done {
        text-decoration: line-through;
        color: grey;
    }
    
</style>