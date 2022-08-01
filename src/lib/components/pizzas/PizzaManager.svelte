<script>
    import NewPizza from "./NewUpdatePizza.svelte";
    import Pizza from "./Pizza.svelte";
    import "./styles.css";

    export let availableToppings = undefined;
    let selection = [];
    let pizzas = [];
    let pizza = undefined;
    let isCreatePizza = false;
    let isUpdatePizza = false;

    let err = "";

    function createNewPizza() {
        pizza = undefined;
        isUpdatePizza = false;
        isCreatePizza = true;
    }

    function addPizza() {
        if (!isValidPizza(pizza)) return;

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
        if (!isValidPizza(pizza)) return;

        isUpdatePizza = false;
        const id = pizzas.indexOf((p) => (p.id = pizza.id));
        console.log(id);
    }

    function isValidPizza(pizza) {
        err = "";

        // check for null or no name pizza
        if (pizza === undefined || pizza.name === "") {
            err = "Please name your pizza";
            return false;
        }

        if (pizza.toppings.length === 0) {
            err = "Pizza must have toppings";
            return false;
        }

        // check for same named pizza
        if (pizzas.find((p) => p.name === pizza.name)) {
            err = "Cannot have duplicate pizza name";
            return false;
        }

        // check for same toppings
        let areSameToppings = false;
        for (let i = 0; i < pizzas.length; i++) {
            const p = pizzas[i];
            if (p.toppings.length !== pizza.toppings.length) continue;

            const p1 = p.toppings.concat().sort();
            const p2 = pizza.toppings.concat().sort();

            for (let j = 0; j < p1.length; j++) {
                if (p1[j] !== p2[j]) continue;
                areSameToppings = true;
                break;
            }

            if (areSameToppings) {
                err = "Pizza cannot have exact same toppings as another pizza";
                break;
            }
        }
        return !areSameToppings;
    }
</script>

<h2>Pizzas</h2>

{#if pizzas !== undefined}
    {#each pizzas as pizza}
        <Pizza
            id={pizza.id}
            bind:value={pizza.name}
            bind:toppings={pizza.toppings}
            bind:group={selection}
        />
    {/each}
{/if}

<p class="error">{err}</p>

<div class="buttons">
    <button on:click={createNewPizza}>Create New Pizza</button>
    <button on:click={removePizza}>Remove Pizza</button>

    {#if pizzas.length > 0 && selection.length > 0}
        <button on:click={updatePizza}>Update Pizza</button>
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
    <NewPizza
        bind:availableToppings
        bind:pizza
        on:updatePizza={updateSelectedPizza}
        bind:err
        isUpdatePizza={true}
    />
{/if}
