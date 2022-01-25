<script>
    import { shoppingCart } from "../stores/shoppingCart";
    import { fauna } from '../stores/fauna';
    const reducer = (previousValue, currentValue) => previousValue + parseFloat(currentValue.price);
    let total;
    let customerName;
    let confirmed = false;

    $: total = $shoppingCart.reduce(reducer, 0);

    function removeItem(idx) {
        shoppingCart.update(n => {
            if (idx === 0) return [...n.slice(1)];
            if (idx === n.length - 1) return [...n.slice(0, n.length - 1)]
            return [...n.slice(0, idx), ...n.slice(idx+1)]
        })
    }

    function submitOrder() {
        console.log(customerName);
        $fauna.client.query(
            $fauna.q.Create(
                $fauna.q.Collection('Orders'),
                { data: { customerName: customerName, details: $shoppingCart, total: total } }
            )
        ).then(() => {
            confirmed = true;
        })
    }
</script>

<section>
    <h2>Shopping Cart</h2>
    <table>
        <thead>
        <tr>
            <th>Item</th>
            <th>Price</th>
            <th></th>
        </tr>
        </thead>
        <tbody>
        {#each $shoppingCart as item, idx}
        <tr>
            <td>{item.name}</td>
            <td>${item.price.toFixed(2)}</td>
            <td><button on:click={() => removeItem(idx)}>Remove</button></td>
        </tr>
        {/each}
        </tbody>
        <tfoot>
        <tr>
            <td>Total</td>
            <td>${total.toFixed(2)}</td>
            <td></td>
        </tr>
        </tfoot>
    </table>
    {#if !confirmed}
    <form on:submit|preventDefault={submitOrder}>
        <label for="name">Name: </label><input type="text" id="name" bind:value={customerName} />
        <input type="submit" />
    </form>
    {:else}
        <div>Thank you very much for your order!</div>
    {/if}
</section>

<style>
    section, table {
        margin: 0 auto;
    }
    table {
        border: 1px solid #000;
        border-collapse: collapse;
    }
    td, th {
        padding: 1rem;
    }
    tr:nth-child(even) {
        background: rgb(172, 170, 170);
    }
    thead, tfoot {
        font-weight: bold;
    }
</style>