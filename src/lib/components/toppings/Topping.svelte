<script>
    import "./styles.css";

    export let id = -1;
    export let value = undefined;
    export let group = undefined;
</script>

<label>
    <input type="checkbox" bind:group {value} class="topping" />
    {value}
</label>
