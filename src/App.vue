<script setup>
import { ref} from 'vue';



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
    
    </section>
  </main>
</template>

