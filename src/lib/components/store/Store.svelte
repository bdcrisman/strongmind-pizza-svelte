<script>
    import PizzaManager from "../pizzas/PizzaManager.svelte";
    import ToppingsManager from "../toppings/ToppingsManager.svelte";
    import "./styles.css";

    let availableToppings;
</script>

<h1>Mr. Ben's Pizza!</h1>
<hr />

<ToppingsManager bind:availableToppings />
<hr />

<PizzaManager {availableToppings} />
