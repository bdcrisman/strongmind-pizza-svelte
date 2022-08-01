<script>
    export let id = -1;
    export let value = undefined;
    export let toppings = [];
    export let group = undefined;
</script>

<div>
    <label class="pizza-simple">
        <input type="checkbox" bind:group {value} class="pizza-input" />
        <span class="pizza-name">{value + ": "}</span>
        {#each toppings as topping}
            {topping.name + ", "}
        {/each}
    </label>
</div>
