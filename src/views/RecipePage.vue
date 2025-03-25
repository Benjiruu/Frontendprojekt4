<template>
  <div class="recipe-page">
    <h1>{{ recipe.name }}</h1>
    <img class="recipe-image" :src="getImagePath(recipe.image)" :alt="recipe.name" />
    <h2>Ingredienser</h2>
    <ul class="no-bullets">
      <li v-for="ingredient in recipe.ingredients" :key="ingredient.name">
        {{ ingredient.name }}: {{ ingredient.quantity }} {{ ingredient.unit }}
      </li>
    </ul>
    <h2>Instruktioner</h2>
    <ul class="instructions-list">
      <li v-for="(step, index) in formattedInstructions" :key="index">
        <span class="step-number">{{ index + 1 }}.</span> {{ step }}
      </li>
    </ul>
    <button @click="goBack">Tillbaka</button>
  </div>
</template>

<script>
import recipes from '../recipes.json';

export default {
  data() {
    return {
      recipe: null,
    };
  },
  computed: {
    formattedInstructions() {
      return this.recipe.instructions.split('.').map(step => step.trim()).filter(step => step);
    },
  },
  created() {
    const recipeId = parseInt(this.$route.params.id);
    this.recipe = Object.values(recipes).flat().find((r) => r.id === recipeId);
  },
  methods: {
    getImagePath(image) {
      return new URL(image, import.meta.url).href;
    },
    goBack() {
      this.$router.go(-1);
    },
  },
};
</script>

<style scoped>
.recipe-page {
  max-width: 800px;
  margin: 0 auto;
  padding: 20px;
  text-align: center;
}

.recipe-image {
  max-width: 50%;
  height: auto;
  display: block;
  margin: 0 auto;
  border-radius: 8px;
}

.no-bullets {
  list-style-type: none;
  padding: 0;
  text-align: center;
}

.instructions-list {
  list-style-type: none;
  padding: 0;
  text-align: center;
}

.instructions-list li {
  display: flex;
  justify-content: center;
  align-items: flex-start;
  margin-bottom: 0.5rem;
}

.step-number {
  font-weight: bold;
  margin-right: 0.5rem;
}

button {
  background-color: #42b983;
  color: white;
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  cursor: pointer;
  margin-top: 20px;
}

button:hover {
  background-color: #369f6e;
}
</style>
