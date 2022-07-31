<script>
    import PizzaManager from "../pizzas/PizzaManager.svelte";
    import ToppingsManager from "../toppings/ToppingsManager.svelte";
    import "./styles.css";

    let toppings;
    let isReady = false;
</script>

<h1>Mr. Ben's Pizza!</h1>
<hr />

<ToppingsManager bind:availableToppings={toppings} bind:isReady />
<hr />

{#if isReady}
    <PizzaManager bind:availableToppings={toppings} />
{/if}
