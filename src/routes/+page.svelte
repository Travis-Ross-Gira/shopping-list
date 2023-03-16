<script lang="ts" context="module">
    type Item = {
      id: string;
      timeCreated: number;
      timeCompleted?: number;
      text: string;
    };

    function nextId() {
      return Math.random().toString(36).substring(2, 15);
    }

    function newShoppingItem(): Item {
      return {
        id: nextId(),
        timeCreated: 0,
        text: '',
      };
    }
  </script>

<script lang="ts">
    import { writable, type Writable } from "svelte/store";
    import { undoStack, transactionCtrl } from '@gira-de/svelte-undo';

    // todo items
    let itemStore: Writable<Record<string, Item>> = writable({});
    $: allItems = Object.values($itemStore);

    // new todo item
    let newItem = newShoppingItem();

    function addDraftItem(){
        newItem.timeCreated = Date.now();
        const itemsDraft = transaction.draft(itemStore);
        itemsDraft[newItem.id] = newItem;
        transaction.commit(`item ${newItem.text} added`);
        newItem = newShoppingItem();
    }

    const myUndoStack = undoStack('undo-stack created');
    const transaction = transactionCtrl(myUndoStack);
</script>

<body>
    <div class="app">
        <div>
            <h3>Undo Stack</h3>
            <button on:click={myUndoStack.undo} disabled={!$myUndoStack.canUndo}>Undo</button>
            <button on:click={myUndoStack.redo} disabled={!$myUndoStack.canRedo}>Redo</button>
            <ul>
                {#each $myUndoStack.actions.slice().reverse() as undoAction}
                  <li
                    class:redo={undoAction.seqNbr > $myUndoStack.selectedAction.seqNbr}
                    class:active={undoAction.seqNbr === $myUndoStack.selectedAction.seqNbr}
                    on:click={() => myUndoStack.goto(undoAction.seqNbr)}
                    on:keydown>
                    {undoAction.msg}
                  </li>
                {/each}
            </ul>
        </div>
        <div>
            <h1>Shopping List</h1>
            <ul>
                <input type="text" bind:value={newItem.text} />
                <button
                    class="transparent"
                    on:click={addDraftItem}
                    disabled={!newItem.text}
                >
                    <span class="material-icons">add</span>
                </button>
                <li>Brot</li>
                <li>Eier</li>
                <li>Käse</li>
                {#each allItems as item}
                    <li>{item.text}</li>
                {/each}
        </div>
    </div>
</body>

<style>
    body {
        background: #C5C5C5;
        text-align: center;
        font-family: "Arial", sans-serif;
        color: #ffffff;
    }

    .app {
        display: grid;
        grid-template-columns: 1fr 2fr;
    }

    h1 {
        font-weight: 800;
    }

    input {
        border-radius: 5px;
        min-width: 60%;
        padding: 20px;
        margin-top: 10px;
    }

    button {
        border: none;
        padding: 20px;
        border-radius: 5px;
        color: #ffffff;
        background-color: #000000;
        transition: all 0.75s ease;
        font-weight: normal;
    }

    ul {
        padding-left: 20px;
        padding-right: 20px;
    }

    li {
        text-align: left;
        list-style: none;
        padding: 20px;
        color: #000000;
        font-weight: 600;
        border-radius: 5px;
        background-color: lightgray;
    }

    button:hover {
        background-color: #ffffff;
        color: #000000;
    }

    .shopping-list {
        display: grid;
        grid-template-columns: 8fr 1fr;
        gap: 2rem;
        padding: 1rem;
    }
</style>