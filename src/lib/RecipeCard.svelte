<script lang="ts">
  import { createEventDispatcher } from "svelte";
  import RecipeCardContent from "./RecipeCardContent.svelte";
  const dispatch = createEventDispatcher();
  export let recipe: App.RecipeCard;
  function ingredientsList(
    recipe: Pick<App.RecipeCard, "ingredients">
  ): [string, string][] {
    return Object.entries(recipe.ingredients);
  }
</script>

<div class="recipe-card">
  <h3>{recipe.name}</h3>
  {#if !recipe.flipped}
    <RecipeCardContent>
      <ul slot="card-content-list">
        {#each ingredientsList(recipe) as [key, val]}
          <li>{key} - {val}</li>
        {/each}
      </ul>

      <a
        slot="link-container"
        href="#/"
        on:click={() => dispatch("flip", { flipped: true })}
        >View instructions</a
      >
    </RecipeCardContent>
  {:else}
    <RecipeCardContent>
      <ul slot="card-content-list">
        {#each recipe.instructions as instruction}
          <li>{instruction}</li>
        {/each}
      </ul>

      <a
        slot="link-container"
        href="#/"
        on:click={() => dispatch("flip", { flipped: false })}
        >Back to ingredients</a
      >
    </RecipeCardContent>
  {/if}
</div>

<style>
  .recipe-card {
    outline: 1px solid gray;
    padding: 10px;
    height: 250px;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
  }
</style>
