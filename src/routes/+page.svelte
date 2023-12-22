<script lang="ts">
  import RecipeCard from "$lib/RecipeCard.svelte";

  export let data;
  $: recipesCards = data.recipes.map<App.RecipeCard>((recipe) => ({
    ...recipe,
    flipped: false,
  }));
  let isRecipeCreateDialogOpen = false;
</script>

<h1>Welcome to Heritage Oven Chronicle</h1>

<section>
  <h2>Recipes</h2>
  <button on:click={() => (isRecipeCreateDialogOpen = true)}
    >Create Recipe</button
  >
  <dialog open={isRecipeCreateDialogOpen}>
    <button on:click={() => (isRecipeCreateDialogOpen = false)}>Close</button>
  </dialog>
  <div class="recipe-grid">
    {#each recipesCards as recipe}
      <RecipeCard
        {recipe}
        on:flip={(e) => (recipe.flipped = e.detail.flipped)}
      />
    {/each}
  </div>
</section>

<style>
  .recipe-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
  }
</style>
