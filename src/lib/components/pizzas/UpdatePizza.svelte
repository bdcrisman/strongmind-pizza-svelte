<script>
    import { createEventDispatcher, onMount } from "svelte";
    import Topping from "../toppings/Topping.svelte";
    import "./styles.css";

    export let availableToppings = undefined;
    export let existingPizza = undefined;
    export let updatedPizza = {
        id: -1,
        name: "",
        toppings: [],
    };

    let selection = [];
    let isDirty = false;
    let err = "";

    const dispatch = createEventDispatcher();

    /**
     * @name addTopping
     * @description Add topping to pizza in update state.
     */
    function addTopping() {
        if (selection.length === 0) return;

        const topping = availableToppings.find((x) => x.name === selection[0]);
        if (existingPizza.toppings.includes(topping)) {
            err = "Cannot have duplicated toppings";
            return;
        }

        err = "";
        updatedPizza.toppings.push(topping);
        updatedPizza.toppings = updatedPizza.toppings;
        isDirty = true;
    }

    /**
     * @name removeTopping
     * @description Removes topping from pizza in update state.
     */
    function removeTopping() {
        if (selection.length === 0) return;

        const topping = availableToppings.find((x) => x.name === selection[0]);
        if (!existingPizza.toppings.includes(topping)) return;
        updatedPizza.toppings = existingPizza.toppings.filter(
            (x) => x !== topping
        );

        isDirty = true;
    }

    /**
     * @name updatePizza
     * @description Updates the pizza.
     */
    function updatePizza() {
        updatedPizza.id = existingPizza.id;

        if (updatedPizza.name === undefined || updatedPizza.name === "") {
            updatedPizza.name = existingPizza.name;
        }

        if (!isDirty) {
            updatedPizza.toppings = existingPizza.toppings;
        }

        updatedPizza = updatedPizza;
        dispatch("updatePizza");
    }

    /**
     * @name init
     * @description Initializes the component.
     */
    function init() {
        updatedPizza = existingPizza;
    }

    // Runs when first rendered to the DOM.
    onMount(async () => {
        init();
    });
</script>

<h3>Update Pizza</h3>

<div>Current name: {existingPizza.name}</div>
<input
    type="text"
    bind:value={updatedPizza.name}
    placeholder="update pizza name..."
/>

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

{#each updatedPizza.toppings as t}
    <ul>
        <li>{t.name}</li>
    </ul>
{/each}

<p class="error">{err}</p>

<div>
    <button on:click={updatePizza} class="add-update-pizza-btn"
        >Update Pizza</button
    >
</div>
