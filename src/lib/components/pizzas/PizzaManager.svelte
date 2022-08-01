<script>
    import NewPizza from "./NewPizza.svelte";
    import UpdatePizza from "./UpdatePizza.svelte";
    import Pizza from "./Pizza.svelte";
    import "./styles.css";

    export let availableToppings = undefined;
    let selection = [];
    let pizzas = [];
    let pizza = undefined;
    let updatedPizza = undefined;
    let isCreatePizza = false;
    let isUpdatePizza = false;

    let err = "";

    function createNewPizza() {
        pizza = undefined;
        isUpdatePizza = false;
        isCreatePizza = true;
    }

    function addPizza() {
        if (!isValidPizza(pizza, false)) return;

        isCreatePizza = false;
        pizza.id = pizzas.length;
        pizzas.push(pizza);
        pizzas = pizzas;
        pizza = undefined;
    }

    function removePizza() {
        pizzas = pizzas.filter((p) => p.name != selection[0]);
    }

    function updatePizza() {
        const result = pizzas.filter((p) => p.name === selection[0]);
        if (result.length === 0) return;

        pizza = result[0];
        isCreatePizza = false;
        isUpdatePizza = true;
    }

    function updateSelectedPizza() {
        if (!isValidPizza(updatedPizza, true)) return;

        pizzas[updatedPizza.id] = updatedPizza;
        isUpdatePizza = false;
    }

    function isValidPizza(inPizza, isUpdate) {
        err = "";

        // check for null or no name pizza
        if (inPizza === undefined || inPizza.name === "") {
            err = "Please name your pizza";
            return false;
        }

        // do we have any toppings?
        if (inPizza.toppings.length === 0) {
            err = "Pizza must have toppings";
            return false;
        }

        // don't care about having the same name or toppings
        // can update anyway
        if (isUpdate) return true;

        // check for same named pizzas
        if (pizzas.find((p) => p.name === inPizza.name)) {
            err = "Cannot have duplicate pizza name";
            return false;
        }

        // check for same toppings
        let haveSameToppings = isPizzaToppingsSame(inPizza);
        return !haveSameToppings;
    }

    function isPizzaToppingsSame(inPizza) {
        let haveSameToppings = false;

        for (let i = 0; i < pizzas.length; i++) {
            const p = pizzas[i];
            if (p.toppings.length !== inPizza.toppings.length) continue;

            const p1 = p.toppings.concat().sort();
            const p2 = inPizza.toppings.concat().sort();

            for (let j = 0; j < p1.length; j++) {
                if (p1[j] !== p2[j]) continue;
                haveSameToppings = true;
                break;
            }

            if (haveSameToppings) {
                err = "Pizza cannot have exact same toppings as another pizza";
                break;
            }
        }
        return haveSameToppings;
    }
</script>

<h2>Pizzas</h2>

{#if pizzas !== undefined}
    {#each pizzas as p}
        <Pizza
            id={p.id}
            bind:value={p.name}
            bind:toppings={p.toppings}
            bind:group={selection}
        />
    {/each}
{/if}

<p class="error">{err}</p>

<div class="buttons">
    <button on:click={createNewPizza}>Create New Pizza</button>
    <button on:click={removePizza}>Remove Pizza</button>

    {#if pizzas.length > 0 && selection.length > 0}
        <button on:click={updatePizza}>Edit Pizza</button>
    {/if}
</div>

<hr />

{#if isCreatePizza}
    <NewPizza
        bind:availableToppings
        bind:pizza
        on:addPizza={addPizza}
        bind:err
    />
{:else if isUpdatePizza}
    <UpdatePizza
        bind:availableToppings
        bind:updatedPizza
        on:updatePizza={updateSelectedPizza}
        bind:err
        existingPizza={pizza}
    />
{/if}
