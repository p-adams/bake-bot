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
      <div class="recipe-card">
        <h3>{recipe.name}</h3>

        {#if !recipe.flipped}<div class="card-content">
            <ul>
              {#each ingredientsList(recipe) as [key, val]}
                <li>{key} - {val}</li>
              {/each}
            </ul>
            <div class="link-container">
              <a href="#/" on:click={() => (recipe.flipped = !recipe.flipped)}
                >View instructions</a
              >
            </div>
          </div>
        {:else}
          <div class="card-content">
            <ul>
              {#each recipe.instructions as instruction}
                <li>{instruction}</li>
              {/each}
            </ul>
            <div class="link-container">
              <a href="#/" on:click={() => (recipe.flipped = !recipe.flipped)}
                >Back to ingredients</a
              >
            </div>
          </div>
        {/if}
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
    padding: 10px;
    height: 250px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }
  .link-container {
    display: flex;
    justify-content: end;
  }
</style>
