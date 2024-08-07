<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'

const recipes = ref([])
const selectedRecipeId = ref(null)
const ingredients = ref([])
const numServings = ref(1)
const servingSize = ref(100)
const nutritionValues = ref(null)

const loadRecipes = async () => {
  const response = await axios.get('/api/recipes')
  recipes.value = response.data
}

const loadIngredients = async () => {
  if (selectedRecipeId.value) {
    const response = await axios.get(`/api/recipes/${selectedRecipeId.value}/ingredients`)
    ingredients.value = response.data
  }
}

const calculateNutrition = async () => {
  const response = await axios.post('/api/calculate-nutrition', {
    recipe_id: selectedRecipeId.value,
    num_servings: numServings.value,
    serving_size: servingSize.value
  })
  nutritionValues.value = response.data
}

onMounted(() => {
  loadRecipes()
})
</script>

<template>
  <div>
    <v-row class="pt-16 pb-4">
      <v-col cols="9" class=" d-flex flex-column align-start justify-between">
        <p class="form-message ml-8">
          Easily calculate the nutritional value of your favorite recipes and meals with just a few
          clicks
        </p>
      </v-col>
      <v-col cols="3" class="pa-0 d-flex flex-column align-start justify-between">
        <div>
          <p class="text-h6 font-weight-regular">Advertisement</p>
        </div>
      </v-col>
    </v-row>

    <v-row>
      <!-- Column for the Recipe Card -->
      <v-col cols="6" class="pa-0">
        <v-container class="mx-0 my-0 pa-0 ml-10">
          <v-card width="860" class="py-8 border rounded-xl" elevation="0">
            <v-row class="pt-6">
              <v-col cols="12" class="px-10  text-subtitle-1 font-weight-bold mt-6 ml-2 ">
                Recipe Name
                <v-select class="rounded-xl mt-3" v-model="selectedRecipeId" :items="recipes" item-title="name"
                  item-value="id" label="Select a recipe" @update:model-value="loadIngredients"></v-select>
              </v-col>
            </v-row>

            <div>
              <v-container class="ml-9 text-subtitle-1  mx-0 pa-0 mt-7">
                <span class="font-weight-bold">Recipe Ingredients </span>
                <v-card border="sm" class="rounded-lg fixed-square mt-3" style="width: 780px" elevation="0">
                  <v-row>
                    <v-col cols="12">
                      <v-list class="scrollable-list ">
                        <v-list-item v-if="ingredients.length === 0"> </v-list-item>
                        <v-list-item class="font-weight-normal" cols="12" md="4" v-for="ingredient in ingredients"
                          :key="ingredient.id">
                          {{ ingredient.pivot.amount }}
                          {{ ingredient.pivot.unit }}
                          {{ ingredient.name }}
                        </v-list-item>
                      </v-list>
                    </v-col>
                  </v-row>
                </v-card>
              </v-container>
            </div>
            <v-row class="mx-7 mt-4">
              <v-col cols="12" sm="6" class="text-subtitle-1 font-weight-bold">
                No. of servings
                <v-text-field v-model.number="numServings" type="number" class="w-70"></v-text-field>
              </v-col>
              <v-col cols="12" sm="6" class="text-subtitle-1 font-weight-bold">
                Serving size
                <v-text-field v-model.number="servingSize" type="number" class="w-70"></v-text-field>
              </v-col>
            </v-row>
            <v-row>
              <v-col cols="12" class="d-flex justify-start ml-7">
                <v-btn height="60" width="250" class="rounded-lg mr-4 text-capitalize bg-teal-darken-4"
                  @click="calculateNutrition">Calculate recipe</v-btn>
              </v-col>
            </v-row>
            <v-row v-if="nutritionValues">
              <v-col cols="12">
                <v-card class="w-full mx-8 bg-transparent border rounded-lg pa-2" elevation="0">
                  <v-card-title>Nutrition Values:</v-card-title>
                  <v-card-text>
                    <p>Calories: {{ nutritionValues.calories }}</p>
                    <p>Fat: {{ nutritionValues.fat }}g</p>
                    <p>Carbs: {{ nutritionValues.carbs }}g</p>
                    <p>Protein: {{ nutritionValues.protein }}g</p>
                  </v-card-text>
                </v-card>
              </v-col>
            </v-row>
          </v-card>
        </v-container>
      </v-col>
      <v-col cols="6" class="pa-0 d-flex align-end flex-column">
        <v-card style="height: 750px; width: 400px; background-color: gray; margin-right: 35px"></v-card>
      </v-col>
    </v-row>
  </div>
</template>

<style>
.fixed-square {
  width: 100px;
  height: 300px;
  overflow-y: scroll;
}

.scrollable-list {
  max-height: 100%;
  overflow-y: auto;
}

.form-message {
  font-weight: 500;
  font-size: 20px;
  border: solid 1px solid;
}



.v-field__input {
  border: none !important;
  outline: none !important;
  border-radius: 10px;
  overflow: hidden;
}
</style>
