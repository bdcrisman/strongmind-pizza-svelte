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
        if (!isValidPizza(pizza)) return;

        // const isAllowed = pizzas.every((p) => {
        //     const matches = p.toppings.filter((t) =>
        //         pizza.toppings.includes(t)
        //     );
        //     console.log(matches);

        //     if (matches.length > 0) return false;
        //     return true;
        // });

        // if (!isAllowed) {
        //     err = "Pizza cannot have exact same toppings as another pizza";
        //     return;
        // }

        // err = "";
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

    function isValidPizza(pizza) {
        err = "";
        if (pizza === undefined || pizza.name === "") {
            err = "Please name your pizza";
            return false;
        }

        if (pizzas.find((p) => p.name === pizza.name)) {
            err = "Cannot have duplicate pizza name";
            return false;
        }

        let areSameToppings = false;
        for (let i = 0; i < pizzas.length; i++) {
            const p = pizzas[i];
            if (p.toppings.length !== pizza.toppings.length) continue;

            const p1 = p.toppings.concat().sort();
            const p2 = p.toppings.concat().sort();

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
