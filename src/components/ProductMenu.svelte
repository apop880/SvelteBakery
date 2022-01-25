<script>   
    import MenuSection from './MenuSection.svelte';
    import { fauna } from '../stores/fauna';

    let loaded = false;
    let categories = [];

    $: {
        if ($fauna.client && !loaded) {
            loaded = true;
            $fauna.client.query(
                $fauna.q.Paginate($fauna.q.Match($fauna.q.Index('all_categories')))
                )           
            .then((res) => {
                categories = res.data;
            })
        }
    }
</script>

<section>
    <h2>Menu</h2>
    {#each categories as category}
        <MenuSection {category} />
    {/each}
</section>