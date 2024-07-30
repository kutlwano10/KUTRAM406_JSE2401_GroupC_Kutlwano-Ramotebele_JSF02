<script>
  import { onMount } from "svelte";
  import { fetchProducts } from '../../api/api';
  import ProductCard from "./ProductCard.svelte";
  import Sort from "../sort.svelte";
  import Filter from "../Filter.svelte";
  import {getCategories} from '../../api/api';

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


  /**
   * Filtering Products
  */
  let filteredProducts = [...products];
  let filterItem = "All categories";
  let searchTerm = "";
  let categories = ["All categories"]; // Initialize with "All categories"

  // Fetch categories on component mount
  onMount(async () => {
    const data = await getCategories();
    categories = ["All categories", ...data];
  });

  function handleFilter(category) {
    filterItem = category;
    applyFilters();
  }

  function handleSearch(event) {
    searchTerm = event.target.value;
    applyFilters();
  }

  function applyFilters() {
    filteredProducts = products.filter((product) => {
      const matchesCategory = filterItem === "All categories" || product.category === filterItem;
      const matchesSearch = product.name.toLowerCase().includes(searchTerm.toLowerCase());
      return matchesCategory && matchesSearch;
    });
  }


</script>

<div class=" md:flex gap-5 justify-center">
  <Filter  {filterItem} {categories} {searchTerm} {handleFilter} {handleSearch} />
  <Sort {sorting} {handleSort} />
</div>

<div class="grid justify-center">
  
  <div
    class="lg:max-h-[130rem] max-w-xl mx-auto grid gap-4 grid-cols-1 lg:grid-cols-4 md:grid-cols-2 items-center lg:max-w-none my-4"
  >
  {#each products as product (product.id)}
    <ProductCard {product}/>

  {/each}
</div>
</div>
