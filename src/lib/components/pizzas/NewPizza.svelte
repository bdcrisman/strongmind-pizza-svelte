<script>
    import { createEventDispatcher } from "svelte";
    import Topping from "../toppings/Topping.svelte";
    import "./styles.css";

    export let availableToppings = undefined;
    export let pizza = {
        id: -1,
        name: "",
        toppings: [],
    };

    export let updated = {
        name: "",
        toppings: [],
    };

    let selection = [];
    let err = "";

    const dispatch = createEventDispatcher();

    function addTopping() {
        if (selection.length === 0) return;

        const topping = availableToppings.find((x) => x.name === selection[0]);
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

        const topping = availableToppings.find((x) => x.name === selection[0]);
        if (!pizza.toppings.includes(topping)) return;
        pizza.toppings = pizza.toppings.filter((x) => x !== topping);
    }

    function setUpdatedToExisting() {
        updated = pizza;
    }

    const addPizza = () => dispatch("addPizza");
</script>

<h3>{"Create New Pizza!"}</h3>

<input type="text" bind:value={pizza.name} placeholder="new pizza name..." />

<div>
    <p>Select from the available toppings...</p>
    <p class="error">{err}</p>
    {#each availableToppings as topping}
        <Topping value={topping.name} bind:group={selection} />
    {/each}
</div>

<div>
    <button on:click={addTopping}>Add Topping</button>
    <button on:click={removeTopping}>Remove Topping</button>
</div>

<h4>Selected Toppings</h4>
{#each pizza.toppings as t}
    <ul>
        <li>{t.name}</li>
    </ul>
{/each}

<p class="error">{err}</p>

<div>
    <button on:click={addPizza} class="add-update-pizza-btn"
        >{"Add Pizza"}</button
    >
</div>
