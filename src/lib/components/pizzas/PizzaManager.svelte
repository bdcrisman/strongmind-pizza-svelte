<script>
    import NewPizza from "./NewPizza.svelte";
    import Pizza from "./Pizza.svelte";
    import "./styles.css";

    export let availableToppings = undefined;
    let selection = [];
    let pizzas = [];
    let pizza = undefined;
    let isCreatePizza = false;

    let err = "";

    function displayAvailableToppings(toppings) {
        return toppings.join(", ");
    }

    function createNewPizza() {
        isCreatePizza = true;
    }

    function addPizza() {
        if (pizza === undefined || pizza.name === "") {
            err = "Please name your pizza";
            return;
        }

        err = "";
        isCreatePizza = false;
        pizzas.push(pizza);
        pizzas = pizzas;

        pizza = undefined;
    }

    function removePizza() {
        pizzas = pizzas.filter((x) => {
            console.log(x);
            !selection.includes(x.name);
        });
    }

    function updatePizza() {}
</script>

<h2>Pizzas</h2>

{#if pizzas !== undefined}
    {#each pizzas as pizza}
        <Pizza
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
