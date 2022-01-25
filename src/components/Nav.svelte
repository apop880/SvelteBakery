<script>
    import { shoppingCart } from '../stores/shoppingCart';
    import { createEventDispatcher } from 'svelte';

	const dispatch = createEventDispatcher();

	function changePage(newPage) {
        if (newPage === "menu" || $shoppingCart.length > 0)
		dispatch('changepage', {
			newPage: newPage
		});
	}
</script>

<nav>
    <h1>Svelte Bakery Demo</h1>
    <a href="#" on:click={() => changePage("menu")}>Menu</a>
    <a href="#" on:click={() => changePage("cart")}>Cart
        {#if $shoppingCart.length > 0}({$shoppingCart.length} items){/if}
    </a>
</nav>

<style>
    nav {
        display: flex;
        color: #FFF;
        background-color: rgb(14, 14, 46);
        flex-direction: column;
        align-items: center;
    }
    @media(min-width: 768px) {
        nav {
            flex-direction: row;
            gap: 3rem;
        }
    }
    h1 {
        flex-basis: 50%;
    }
    a {
        text-decoration: none;
        color: #FFF;
        font-weight: bold;
    }
    a:hover {
        text-decoration: underline;
    }

</style>