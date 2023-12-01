<script lang="ts">
  export let data;
</script>

<h1>Welcome to Heritage Oven Chronicle</h1>

<section>
  <h2>Recipes</h2>
  <div class="recipe-grid">
    {#each data.recipes as recipe}
      <div class="recipe-card">
        <h3>{recipe.name}</h3>
      </div>
    {/each}
  </div>
</section>

<style>
  .recipe-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
  }
  .recipe-card {
    outline: 1px solid gray;
  }
</style>
