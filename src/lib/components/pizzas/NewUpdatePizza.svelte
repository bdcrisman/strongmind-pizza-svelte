<script>
    import { createEventDispatcher } from "svelte";
    import Topping from "../toppings/Topping.svelte";
    import "./styles.css";

    export let err = "";
    export let isUpdatePizza = false;
    export let availableToppings = undefined;
    export let pizza = {
        id: -1,
        name: "",
        toppings: [],
    };

    const dispatch = createEventDispatcher();
    let selection = [];

    function addTopping() {
        if (selection.length === 0) return;

        const topping = selection[0];
        if (pizza.toppings.includes(topping)) {
            err = "Cannot have duplicated toppings";
            return;
        }

        err = "";
        pizza.toppings.push(topping);
        pizza.toppings = pizza.toppings;
    }

    function removeTopping() {
        if (selection.length === 0) return;

        const topping = selection[0];
        if (!pizza.toppings.includes(topping)) return;

        pizza.toppings = pizza.toppings.filter((x) => x !== topping);
    }

    const addPizza = () => dispatch("addPizza");
    const updatePizza = () => dispatch("updatePizza");
</script>

<h3>{isUpdatePizza ? `Update ${pizza.name}` : "Create New Pizza!"}</h3>
<input type="text" bind:value={pizza.name} placeholder="new pizza name..." />

<div>
    <p>Select from the available toppings...</p>
    <p class="error">{err}</p>
    {#each availableToppings as topping}
        <Topping value={topping} bind:group={selection} />
    {/each}
</div>

<div>
    <button on:click={addTopping}>Add Topping</button>
    <button on:click={removeTopping}>Remove Topping</button>
</div>

<h4>Selected Toppings</h4>
{#each pizza.toppings as t}
    <ul>
        <li>{t}</li>
    </ul>
{/each}

<p class="error">{err}</p>

<div>
    <button
        on:click={isUpdatePizza ? updatePizza : addPizza}
        class="add-update-pizza-btn"
        >{isUpdatePizza ? "Update Pizza" : "Add Pizza"}</button
    >
</div>
