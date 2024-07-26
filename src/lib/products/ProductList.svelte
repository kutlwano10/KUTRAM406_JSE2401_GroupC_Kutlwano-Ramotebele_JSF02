<script>
  import { onMount } from "svelte";
  import { fetchProducts } from '../../api/api';
  import ProductCard from "./ProductCard.svelte";

  let products = []

  onMount(async ()=>{
    try {
      products =await fetchProducts()

      
    } catch (error) {
      console.error(`There is something wrong`, error)
      
    }
  })
</script>

<div class="grid justify-center">
  <div
    class="lg:max-h-[130rem] max-w-xl mx-auto grid gap-4 grid-cols-1 lg:grid-cols-4 md:grid-cols-2 items-center lg:max-w-none my-4"
  >
  {#each products as product (product.id)}
    <ProductCard {product}/>

  {/each}
</div>
</div>
