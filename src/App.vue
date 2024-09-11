<script setup>
import { ref, onMounted, watch } from 'vue';

const recipeName = ref('');
const newIngredient = ref('');
const newDirection = ref('');

const recipe = ref([]);

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

const removeRecipeItem = (index) => {
  recipe.value.splice(index, 1);
};

onMounted(() => {
  recipeName.value = localStorage.getItem('recipeName') || '';
  recipe.value = JSON.parse(localStorage.getItem('recipe')) || [];
});


watch(recipeName, (newVal) => {
  localStorage.setItem('recipeName', newVal);
});


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
        <input type="text" placeholder="e.g., Pizza" v-model="recipeName" />

        <h4>Add Ingredient</h4>
        <input type="text" placeholder="e.g., Tomatoes" v-model="newIngredient" />

        <h4>Add Directions</h4>
        <input type="text" placeholder="e.g., Blend the tomatoes finely" v-model="newDirection" />

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

