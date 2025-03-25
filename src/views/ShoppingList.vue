<template>
  <div class="shopping-list">
    <h1>Inköpslista</h1>
    <table>
      <thead>
        <tr>
          <th>Ingrediens</th>
          <th>Antal</th>
          <th>Enhet</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="ingredient in sortedIngredients" :key="ingredient.name">
          <td>{{ ingredient.name }}</td>
          <td>{{ formatQuantity(ingredient.quantity) * portions }}</td>
          <td>{{ ingredient.unit }}</td>
        </tr>
      </tbody>
    </table>
    <button @click="goBack">Tillbaka</button>
  </div>
</template>

<script>
export default {
  computed: {
    ingredients() {
      const recipes = JSON.parse(this.$route.query.selectedRecipes);
      const allIngredients = [
        ...recipes.appetizer.ingredients,
        ...recipes.mainCourse.ingredients,
        ...recipes.dessert.ingredients,
      ];
      return allIngredients;
    },
    sortedIngredients() {
      return this.ingredients.sort((a, b) => a.name.localeCompare(b.name));
    },
    portions() {
      return this.$route.query.portions || 1;
    },
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
      this.$router.push({
        name: 'MenuSummary',
        query: {
          selectedRecipes: this.$route.query.selectedRecipes,
          portions: this.portions,
        },
      });
    },
  },
};
</script>

<style scoped>
.shopping-list {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  background-color: #f9f9f9;
  border-radius: 8px;
}

table {
  width: 100%;
  border-collapse: collapse;
  margin-bottom: 20px;
}

th, td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

th {
  background-color: #42b983;
  color: white;
}

button {
  background-color: #42b983;
  color: white;
  border: none;
  padding: 0.5rem 1rem;
  border-radius: 4px;
  cursor: pointer;
}

button:hover {
  background-color: #369f6e;
}
</style>