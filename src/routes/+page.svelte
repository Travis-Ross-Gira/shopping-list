<script lang="ts">
    let newItem = '';

    let shoppingList: string[] = [
        'Brot',
        'Käse',
        'Milch',
        'Eier',
    ];

    function addItem(){
        shoppingList = [...shoppingList, newItem];
    }

    function removeItem(index: number){
        shoppingList.splice(index, 1);
        shoppingList = shoppingList;
    }
</script>

<body>
    <h1>Einkaufsliste</h1>
    <img src="https://lerneprogrammieren.de/wordpress/wp-content/uploads/einkaufswagen.png" height="35px" alt="Einkaufswagen Icon"/>
    <div>
        <input id="userInput" type="text" placeholder="Neuer Eintrag" bind:value={newItem} />
        <button id="enterButton" on:click={addItem}>Hinzufügen</button>
        <ul id="shoppingList">
            {#each shoppingList as item, index}
                <li>{item}</li>
                <button on:click={() => removeItem(index)}>❌</button>
            {/each}
        </ul>
    </div>
</body>

<style>
    body {
        background: #1d1e22;
        text-align: center;
        font-family: "Arial", sans-serif;
        color: #ffffff;
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

    #enterButton {
        border: none;
        padding: 20px;
        border-radius: 5px;
        color: #ffffff;
        background-color: #4eb9cd;
        transition: all 0.75s ease;
        font-weight: normal;
    }

    ul {
        padding-left: 20px;
        padding-right: 20px;
    }

    li {
        text-align: left;
        margin-top: 20px;
        list-style: none;
        padding: 20px;
        color: #ffffff;
        text-transform: capitalize;
        font-weight: 600;
        border-radius: 5px;
        margin-bottom: 10px;
        background: #4eb9cd;
        transition: all 0.75s ease;
    }

    li:hover {
        background: #76cfe0;
    }
</style>