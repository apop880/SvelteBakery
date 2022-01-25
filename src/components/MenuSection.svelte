<script>
    import MenuItem from './MenuItem.svelte';
    import { onMount } from 'svelte';
    import { fauna } from '../stores/fauna';
    export let category;
    let [ ref, name ] = category;

    let items = [];

    onMount(async () => {
        let x = await $fauna.client.query(
            $fauna.q.Paginate($fauna.q.Match($fauna.q.Index('product_by_category'), ref))
        );
        items = x.data;
    })
</script>

<div>
    <h3>{name}</h3>
    <div class="items">
        {#each items as item}
            <MenuItem {item} />
        {/each}
    </div>
</div>

<style>
    .items {
        display: flex;
        gap: 2rem;
        justify-content: center;
        flex-wrap: wrap;
    }
</style>
