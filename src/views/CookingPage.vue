<template>
  <div class="cooking-page">
    <header class="page-header">
      <h1>Recept</h1>
    </header>
    <div class="content">
      <div v-if="selectedMenu">
        <div class="menu-section">
          <h2>Förrätt</h2>
          <img class="recipe-image" :src="selectedMenu.appetizer.image" alt="Förrätt" />
          <h3>Ingredienser</h3>
          <ul class="no-bullets">
            <li v-for="ingredient in selectedMenu.appetizer.ingredients" :key="ingredient.name">
              - {{ ingredient.name }}: {{ formatQuantity(ingredient.quantity) * portions }} {{ ingredient.unit }}
            </li>
          </ul>
          <h3>Instruktioner</h3>
          <ul class="instructions-list">
            <li v-for="(step, index) in formattedAppetizerInstructions" :key="index">
              <span class="step-number">{{ index + 1 }}.</span> {{ step }}
            </li>
          </ul>
        </div>
        <div class="menu-section">
          <h2>Huvudrätt</h2>
          <img class="recipe-image" :src="selectedMenu.mainCourse.image" alt="Huvudrätt" />
          <h3>Ingredienser</h3>
          <ul class="no-bullets">
            <li v-for="ingredient in selectedMenu.mainCourse.ingredients" :key="ingredient.name">
              - {{ ingredient.name }}: {{ formatQuantity(ingredient.quantity) * portions }} {{ ingredient.unit }}
            </li>
          </ul>
          <h3>Instruktioner</h3>
          <ul class="instructions-list">
            <li v-for="(step, index) in formattedMainCourseInstructions" :key="index">
              <span class="step-number">{{ index + 1 }}.</span> {{ step }}
            </li>
          </ul>
        </div>
        <div class="menu-section">
          <h2>Efterrätt</h2>
          <img class="recipe-image" :src="selectedMenu.dessert.image" alt="Efterrätt" />
          <h3>Ingredienser</h3>
          <ul class="no-bullets">
            <li v-for="ingredient in selectedMenu.dessert.ingredients" :key="ingredient.name">
              - {{ ingredient.name }}: {{ formatQuantity(ingredient.quantity) * portions }} {{ ingredient.unit }}
            </li>
          </ul>
          <h3>Instruktioner</h3>
          <ul class="instructions-list">
            <li v-for="(step, index) in formattedDessertInstructions" :key="index">
              <span class="step-number">{{ index + 1 }}.</span> {{ step }}
            </li>
          </ul>
        </div>
      </div>
      <button @click="goBack">Tillbaka</button>
    </div>
  </div>
</template>

<script>
export default {
  computed: {
    selectedMenu() {
      return this.$route.query.selectedRecipes ? JSON.parse(this.$route.query.selectedRecipes) : null;
    },
    portions() {
      return this.$route.query.portions || 1;
    },
    formattedAppetizerInstructions() {
      return this.selectedMenu.appetizer.instructions.split('. ').filter(step => step);
    },
    formattedMainCourseInstructions() {
      return this.selectedMenu.mainCourse.instructions.split('. ').filter(step => step);
    },
    formattedDessertInstructions() {
      return this.selectedMenu.dessert.instructions.split('. ').filter(step => step);
    }
  },
  methods: {
    formatQuantity(quantity) {
      if (typeof quantity === 'string' && quantity.includes('/')) {
        const [numerator, denominator] = quantity.split('/').map(Number);
        return numerator / denominator;
      }
      return Number(quantity) || 0;
    },
    goBack() {
      this.$router.push({ name: 'MenuSummary', query: { selectedRecipes: this.$route.query.selectedRecipes, portions: this.portions } });
    }
  }
};
</script>

<style scoped>
.cooking-page {
  width: 100%;
  margin: 0;
  padding: 20px;
}

.page-header {
  text-align: center;
  margin-bottom: 30px;
}

.page-header h1 {
  font-size: 2.5rem;
  margin: 0;
}

.content {
  text-align: left;
  margin-left: 200px; 
  max-width: 600px; 
}

.menu-section {
  margin-bottom: 30px;
}

.menu-section h2 {
  margin-left: 0;
}

.recipe-image {
  width: 100%;
  max-width: 300px;
  height: auto;
  display: block;
  margin: 10px 0;
  border-radius: 8px;
}

.no-bullets {
  list-style-type: none;
  padding: 0;
  margin: 0;
}

.instructions-list {
  list-style-type: none;
  padding: 0;
  margin: 0;
  text-align: left; 
}

.instructions-list li {
  margin-bottom: 1rem; 
  display: block;
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

@media (max-width: 768px) {
  h1 {
    font-size: 1.5rem;
  }

  h2 {
    font-size: 1.2rem;
  }

  h3 {
    font-size: 1rem;
  }

  p {
    font-size: 1rem;
  }

  button {
    padding: 0.5rem;
    font-size: 0.9rem;
  }

  .recipe-image {
    max-width: 100%;
    height: auto;
  }
}
</style>