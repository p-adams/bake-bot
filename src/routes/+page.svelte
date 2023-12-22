<script lang="ts">
  import RecipeCard from "$lib/RecipeCard.svelte";

  export let data;
  $: recipesCards = data.recipes.map<App.RecipeCard>((recipe) => ({
    ...recipe,
    flipped: false,
  }));
  let isRecipeCreateDialogOpen = false;
  let recipeData: App.Recipe = {
    name: "",
    ingredients: {
      starter: "1 cup active sourdough starter",
      water: "1 1/2 cups warm water",
      flour: "4 cups bread flour",
      salt: "1 1/2 teaspoons salt",
    },
    instructions: [],
  };
  let ingredient = {
    name: "",
    amount: "",
  };
  let instruction = "";
  function handleNewIngredient() {
    recipeData = {
      ...recipeData,
      ingredients: {
        ...recipeData.ingredients,
        [ingredient.name]: ingredient.amount,
      },
    };
    ingredient = { name: "", amount: "" };
  }
  function handleNewInstruction() {
    recipeData.instructions = [...recipeData.instructions, instruction];
    instruction = "";
  }
</script>

<h1>Welcome to Heritage Oven Chronicle</h1>

<section>
  <h2>Recipes</h2>
  <button on:click={() => (isRecipeCreateDialogOpen = true)}
    >Create Recipe</button
  >
  <dialog open={isRecipeCreateDialogOpen}>
    <form>
      <label>
        Name
        <input bind:value={recipeData.name} />
      </label>
      <label>
        Ingredients
        <ul>
          {#each Object.entries(recipeData.ingredients) as [ingredient, amount]}
            <li>{ingredient} - {amount}</li>
          {/each}
        </ul>

        <div>
          <label>
            name
            <input bind:value={ingredient.name} />
          </label>
          <label>
            amount
            <input bind:value={ingredient.amount} />
          </label>
        </div>
        <button on:click={() => handleNewIngredient()}>add ingredient</button>
      </label>
      <label>
        Instructions
        <ul>
          {#each recipeData.instructions as instruction}
            <li>{instruction}</li>
          {/each}
        </ul>
        <input bind:value={instruction} />
        <button on:click={() => handleNewInstruction()}>add instruction</button>
      </label>

      <button on:click={() => (isRecipeCreateDialogOpen = false)}>Close</button>
    </form>
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
  dialog form {
    display: flex;
    flex-direction: column;
  }
</style>
