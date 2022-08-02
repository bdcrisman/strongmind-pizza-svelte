<script>
    import { onMount } from "svelte";
    import Topping from "./Topping.svelte";
    import "./styles.css";

    const defaultToppingNames = ["Marinara Sauce", "Cheese"];

    export let availableToppings = [];
    let selection = [];
    let inputTopping;
    let topping = undefined;
    let err = "";

    /**
     * @name addTopping
     * @description Add topping to the toppings list.
     */
    function addTopping() {
        if (inputTopping === undefined || inputTopping === "") return;

        for (let i = 0; i < availableToppings.length; i++) {
            if (
                availableToppings[i].name.toLowerCase() ===
                inputTopping.toLowerCase()
            ) {
                err = "Cannot have duplicate toppings";
                return;
            }
        }

        err = "";

        const t = {
            id: availableToppings.length,
            name: inputTopping,
        };

        availableToppings.push(t);
        availableToppings = availableToppings;
    }

    /**
     * @name removeTopping
     * @description Remove selected topping from the toppings list.
     */
    function removeTopping() {
        if (selection.length === 0) return;

        availableToppings = availableToppings.filter(
            (x) => !selection.includes(x.name)
        );
    }

    /**
     * @name updateTopping
     * @description Updates the selected topping.
     */
    function updateTopping() {
        if (selection.length === 0) return;
        if (inputTopping === undefined || inputTopping === "") return;

        const items = availableToppings.filter((x) => x.name === selection[0]);
        if (items === undefined || items.length === 0) return;

        items[0].name = inputTopping;
        availableToppings = availableToppings;
    }

    /**
     * @name init
     * @description Initializes the component.
     */
    function init() {
        for (let i = 0; i < defaultToppingNames.length; i++) {
            availableToppings.push({
                id: i,
                name: defaultToppingNames[i],
            });
        }
    }

    // Runs when first rendered to the DOM.
    onMount(async () => {
        init();
    });
</script>

<h2>Toppings</h2>
<p class="error">{err}</p>

<div>
    <input type="text" bind:value={inputTopping} placeholder="new topping..." />
</div>

{#each availableToppings as topping}
    <Topping value={topping.name} bind:group={selection} />
{/each}

<div class="buttons">
    <button on:click={addTopping}>Add Topping</button>
    <button on:click={removeTopping}>Remove Topping</button>

    {#if availableToppings.length > 0 && selection.length > 0}
        <button on:click={updateTopping}>Edit Topping</button>
    {/if}
</div>
