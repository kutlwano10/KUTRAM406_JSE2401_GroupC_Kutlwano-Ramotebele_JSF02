<script>
  import { onMount } from "svelte";
  import { fetchProducts } from '../../api/api';
  import ProductCard from "./ProductCard.svelte";
  import Sort from "../sort.svelte";

  let products = []

  onMount(async ()=>{
    try {
      products =await fetchProducts()

      
    } catch (error) {
      console.error(`There is something wrong`, error)
      
    }
  })

  /**
   * function for sorting the Products
  */
 let defaultProducts = [...products]
 let sorting = 'default'
  function handleSort(event) {
    const value = event.target.value;
    sorting = value;

    if (value === "low") {
      products = products.slice().sort((a, b) => a.price - b.price);
    } else if (value === "high") {
      products = products.slice().sort((a, b) => b.price - a.price);
    } else {
      // Handle default sorting if needed
      products = [...defaultProducts]
    }
  }


</script>

<div class="grid justify-center">
  <Sort {sorting} {handleSort} />
  <div
    class="lg:max-h-[130rem] max-w-xl mx-auto grid gap-4 grid-cols-1 lg:grid-cols-4 md:grid-cols-2 items-center lg:max-w-none my-4"
  >
  {#each products as product (product.id)}
    <ProductCard {product}/>

  {/each}
</div>
</div>
