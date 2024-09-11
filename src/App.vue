<script setup>
import { ref, onMounted, watch } from 'vue';

// Recipe information
const recipeName = ref('');
const newIngredient = ref('');
const newDirection = ref('');

// Recipe list
const recipe = ref([]);

// Function to add a new recipe item (ingredient + direction)
const addRecipe = () => {
  if (newIngredient.value.trim() !== '' && newDirection.value.trim() !== '') {
    recipe.value.push({
      ingredient: newIngredient.value,
      direction: newDirection.value,
    });
    // Clear the input fields after adding
    newIngredient.value = '';
    newDirection.value = '';
  }
};

// Function to remove an item from the recipe list
const removeRecipeItem = (index) => {
  recipe.value.splice(index, 1);
};

// On mounted, load data from localStorage
onMounted(() => {
  recipeName.value = localStorage.getItem('recipeName') || '';
  recipe.value = JSON.parse(localStorage.getItem('recipe')) || [];
});

// Watch for changes in `recipeName` and save to localStorage
watch(recipeName, (newVal) => {
  localStorage.setItem('recipeName', newVal);
});

// Watch for changes in the recipe list and save to localStorage
watch(recipe, (newVal) => {
  localStorage.setItem('recipe', JSON.stringify(newVal));
}, { deep: true });


</script>

<template>
  <main class="app">
    <section>
      <h2 class="title">
        Welcome to Recipe Builder
      </h2>
    </section>

    <section class="create-recipe">
      <h3>Create a Recipe</h3>
      <form @submit.prevent="addRecipe">
        <h4>What's the name of your recipe?</h4>
        <input type="text" placeholder="e.g., Spaghetti Bolognese" v-model="recipeName" />

        <h4>Add Ingredient</h4>
        <input type="text" placeholder="e.g., Tomatoes" v-model="newIngredient" />

        <h4>Add Directions</h4>
        <input type="text" placeholder="e.g., Chop the tomatoes finely" v-model="newDirection" />

        <input type="submit" value="Add to Recipe" />
      </form>
    </section>

    <section class="recipe-list">
      <h3>Recipe: {{ recipeName }}</h3>
      <div class="list">
        <div v-for="(item, index) in recipe" :key="index" class="recipe-item">
          <div class="recipe-content">
            <strong>Ingredient:</strong>
            <input type="text" v-model="item.ingredient" />

            <strong>Direction:</strong>
            <input type="text" v-model="item.direction" />
          </div>

          <div class="actions">
            <button class="delete" @click="removeRecipeItem(index)">Delete</button>
          </div>
        </div>
      </div>
    </section>
  </main>
</template>

