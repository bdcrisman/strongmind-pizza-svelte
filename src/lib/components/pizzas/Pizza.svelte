<script>
    export let value = undefined;
    export let toppings = [];
    export let group = undefined;
</script>

<label class="pizza">
    <input type="checkbox" bind:group {value} class="pizza-input" />
    {value}
</label>
