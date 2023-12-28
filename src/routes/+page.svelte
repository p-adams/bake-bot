<script lang="ts">
  import RecipeCard from "$lib/RecipeCard.svelte";

  export let data;
  $: recipes = data.recipes;
  $: recipesCards = recipes.map<App.RecipeCard>((recipe) => ({
    ...recipe,
    flipped: false,
  }));
  let isRecipeCreateDialogOpen = false;
  let recipeData: App.Recipe = {
    name: "",
    ingredients: {
      sugar: "1 cup",
      flour: "3 cup",
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
  function removeIngredient(ingredientKey: string) {
    const { [ingredientKey]: deletedKey, ...updatedIngredients } =
      recipeData.ingredients;
    recipeData = {
      ...recipeData,
      ingredients: {
        ...updatedIngredients,
      },
    };
  }
  function removeInstruction(ingredientKey: string) {
    recipeData.instructions = recipeData.instructions.filter(
      (i) => i !== ingredientKey
    );
  }
  function createRecipe() {
    recipes = [...recipes, recipeData];
    isRecipeCreateDialogOpen = false;
    recipeData = {
      name: "",
      ingredients: {},
      instructions: [],
    };
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
            <li class="list-item">
              <div>{ingredient} - {amount}</div>
              <button on:click={() => removeIngredient(ingredient)}
                >remove</button
              >
            </li>
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
            <li>
              {instruction}
              <button on:click={() => removeInstruction(instruction)}
                >remove</button
              >
            </li>
          {/each}
        </ul>
        <input bind:value={instruction} />
        <button on:click={() => handleNewInstruction()}>add instruction</button>
      </label>

      <button on:click={() => (isRecipeCreateDialogOpen = false)}>Close</button>
      <button on:click={() => createRecipe()}>Create</button>
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
