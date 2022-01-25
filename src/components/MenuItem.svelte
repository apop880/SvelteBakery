<script>
    import { shoppingCart } from "../stores/shoppingCart";
    export let item;
    let [ name, price ] = item;
    function addToCart() {
      shoppingCart.update(n => {
        return [ ...n, {name: name, price: price}];
      });
    }
</script>

<div>
<h4>{name}</h4>
<strong> ${price.toFixed(2)}</strong>
    <button on:click={addToCart}>
    Add to cart
    </button>
</div>

<style>
    div {
        display: flex;
        flex-direction: column;
        width: 90%;
        border: 1px solid #000;
        border-radius: 5px;
    }

    @media(min-width: 768px) {
        div {
            width: 15%;
        }
    }

    button {
        width: fit-content;
        margin: 1rem auto;
    }
</style>