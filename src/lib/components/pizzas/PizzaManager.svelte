<script>
    import Topping from "../toppings/Topping.svelte";
    import "./styles.css";

    export let availableToppings = undefined;
    let isCreatePizza = false;
    let newPizzaName = undefined;
    let selection = [];
    let newToppings = [];

    function displayAvailableToppings() {
        return availableToppings ? availableToppings.join(", ") : "";
    }

    function createNewPizza() {
        isCreatePizza = true;
    }
    function addPizza() {}
    function removePizza() {}
    function updatePizza() {}
    function addTopping() {
        if (!isCreatePizza) {
            return;
        }
    }
    function removeTopping() {}
</script>

<h2>Pizzas</h2>
<div>{displayAvailableToppings()}</div>
<div class="buttons">
    <button on:click={createNewPizza}>Create New Pizza</button>
    <button on:click={addTopping}>Add Topping</button>
    <button on:click={removeTopping}>Remove Topping</button>
    <button on:click={addPizza}>Add Pizza</button>
    <button on:click={removePizza}>Remove Pizza</button>
    <button on:click={updatePizza}>Update Pizza</button>
</div>

<hr />

{#if isCreatePizza}
    <div class="new-pizza-section">
        <h3>Create New Pizza!</h3>
        <input
            type="text"
            bind:value={newPizzaName}
            placeholder="new pizza name..."
        />
        {#each availableToppings as topping}
            <Topping value={topping} bind:group={selection} />
        {/each}

        <h4>Selected Toppings</h4>
    </div>
{/if}
