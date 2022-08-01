<script>
    export let value = undefined;
    export let toppings = [];
    export let group = undefined;
    export let displayAll = false;
</script>

{#if displayAll}
    <div class="pizza-all">
        <h3>{value}</h3>
        <ul>
            {#each toppings as topping}
                <li>{topping}</li>
            {/each}
        </ul>
    </div>
{:else}
    <div>
        <label class="pizza-simple">
            <input type="checkbox" bind:group {value} class="pizza-input" />
            <span class="pizza-name">{value + ": "}</span>
            {#each toppings as topping}
                {topping + ", "}
            {/each}
        </label>
    </div>
{/if}
