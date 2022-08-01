<script>
    import Topping from "./Topping.svelte";
    import "./styles.css";

    export let availableToppings = ["Marinara Sauce", "Cheese"];
    export let isReady = false;

    let selection = [];
    let inputTopping;
    let err = "";

    function addTopping() {
        for (let i = 0; i < availableToppings.length; i++) {
            if (
                availableToppings[i].toLowerCase() ===
                inputTopping.toLowerCase()
            ) {
                console.log("Cannot have duplicate toppings");
                err = "Cannot have duplicate toppings";
                return;
            }
        }

        err = "";
        availableToppings.push(inputTopping);
        availableToppings = availableToppings;
    }

    function removeTopping() {
        availableToppings = availableToppings.filter(
            (x) => !selection.includes(x)
        );
    }

    function updateTopping() {
        if (selection.length === 0) return;

        const item = selection[0];
        for (let i = 0; i < availableToppings.length; i++) {
            if (item === availableToppings[i]) {
                console.log(availableToppings[i], inputTopping);
                availableToppings[i] = inputTopping;
                return;
            }
        }
    }

    function setIsReady() {
        isReady = true;
    }
</script>

<h2>Toppings</h2>
<p class="error">{err}</p>

<div>
    <input type="text" bind:value={inputTopping} placeholder="new topping..." />
</div>

{#each availableToppings as topping}
    <Topping value={topping} bind:group={selection} />
{/each}

<div class="buttons">
    <button on:click={addTopping}>Add Topping</button>
    <button on:click={removeTopping}>Remove Topping</button>
    <button on:click={updateTopping}>Update Topping</button>
</div>

{#if !isReady}
    {setIsReady()}
{/if}
