<script>
	import ProductMenu from "./components/ProductMenu.svelte";
	import ShoppingCart from "./components/ShoppingCart.svelte";
	import Nav from "./components/Nav.svelte";
	import { fauna } from './stores/fauna';

	let currentPage = "menu";

	function connectToFauna() {
		connectToFauna = () => {};
		fauna.set(
			{
				client: new window.faunadb.Client({ domain: 'db.us.fauna.com', scheme: 'https', secret: "INSERT_SECRET_HERE" }),
				q: window.faunadb.query
			}
		)
	}

	function handlePageChange(e) {
		currentPage = e.detail.newPage;
	}
</script>

<main>
<Nav on:changepage={handlePageChange} />
{#if currentPage === "menu"}
	<ProductMenu />
{:else if currentPage === "cart"}
	<ShoppingCart />
{/if}
</main>

<svelte:head>
	<script src="//cdn.jsdelivr.net/npm/faunadb@latest/dist/faunadb-min.js" on:load={connectToFauna}></script>
</svelte:head>

<style>
	main {
		text-align: center;
		padding: 1em;
		margin: 0 auto;
	}
</style>