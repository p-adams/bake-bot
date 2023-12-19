<script lang="ts">
  export let data;
  $: recipesCards = data.recipes.map<App.RecipeCard>((recipe) => ({
    ...recipe,
    flipped: false,
  }));
  function ingredientsList(
    recipe: Pick<App.RecipeCard, "ingredients">
  ): [string, string][] {
    return Object.entries(recipe.ingredients);
  }
</script>

<h1>Welcome to Heritage Oven Chronicle</h1>

<section>
  <h2>Recipes</h2>
  <div class="recipe-grid">
    {#each recipesCards as recipe}
      <!-- svelte-ignore a11y-no-static-element-interactions -->
      <!-- svelte-ignore a11y-click-events-have-key-events -->
      <div
        class="recipe-card"
        on:click={() => (recipe.flipped = !recipe.flipped)}
      >
        <h3>{recipe.name}</h3>
        <ul>
          {#each ingredientsList(recipe) as [key, val]}
            <li>{key} - {val}</li>
          {/each}
        </ul>
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
