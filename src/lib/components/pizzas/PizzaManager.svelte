<script>
    import Topping from "../toppings/Topping.svelte";
    import NewPizza from "./NewPizza.svelte";
    import Pizza from "./Pizza.svelte";
    import "./styles.css";

    export let availableToppings = undefined;
    let pizzas = [];
    let isCreatePizza = false;
    let pizza = undefined;
    let err = "";

    function displayAvailableToppings(toppings) {
        return toppings.join(", ");
    }

    function createNewPizza() {
        isCreatePizza = true;
    }

    function addPizza() {
        if (pizza === undefined || pizza.name === "") {
            err = "Pizza needs a name";
            return;
        }

        err = "";
        isCreatePizza = false;
        pizzas.push(pizza);
        pizzas = pizzas;
    }

    function removePizza() {}

    function updatePizza() {}
</script>

<h2>Pizzas</h2>

{#if pizzas !== undefined}
    {#each pizzas as pizza}
        {pizza.name}
    {/each}
{/if}

<p class="error">{err}</p>

<div class="buttons">
    <button on:click={createNewPizza}>Create New Pizza</button>
    <button on:click={removePizza}>Remove Pizza</button>
    <button on:click={updatePizza}>Update Pizza</button>
</div>

<hr />

{#if isCreatePizza}
    <NewPizza
        bind:availableToppings
        bind:pizza
        on:addPizza={addPizza}
        bind:err
    />
{/if}
