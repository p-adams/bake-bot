<script lang="ts">
  export let data;
  function ingredientsList(
    recipe: Pick<App.Recipe, "ingredients">
  ): [string, string][] {
    return Object.entries(recipe.ingredients);
  }
</script>

<h1>Welcome to Heritage Oven Chronicle</h1>

<section>
  <h2>Recipes</h2>
  <div class="recipe-grid">
    {#each data.recipes as recipe}
      <div class="recipe-card">
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
