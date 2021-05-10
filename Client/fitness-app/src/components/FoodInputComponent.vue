<template>
  <div class="container">
    <div class="field">
      <label for class="label">Food name and description:</label>
      <div class="control">
        <input type="text" class="input" v-model.trim="input.description" />
      </div>
    </div>
    <div class="field">
      <label for class="label">Type of food:</label>
    </div>
    <div class="field">
      <div class="control is-expanded">
        <div class="select w-100">
          <select class="w-100" v-model="input.foodType">
            <option v-for="type in getFoodTypes" :key="type" :value="type">
              {{ type }}
            </option>
          </select>
        </div>
      </div>
    </div>
    <div class="field">
      <label class="label">How much food:</label>
    </div>
    <div class="field has-addons">
      <div class="control is-expanded">
        <input type="text" class="input" v-model="input.grams" />
      </div>
      <p class="control">
        <a class="button is-static">grams</a>
      </p>
    </div>
    <div class="field">
      <label class="label">Calories:</label>
      <div class="control">
        <input type="text" class="input" v-model.number="input.calories" />
      </div>
    </div>
    <div class="field is-grouped is-grouped-right">
      <label class="checkbox">
        <input type="checkbox" v-model="input.public" />
        Share this accomplishment
      </label>
    </div>
    <div class="field is-grouped is-grouped-centered">
      <div class="control">
        <button
          :disabled="$v.input.$invalid"
          class="button is-link"
          @click="addButtonClick()"
        >
          Add to your daily intake
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from "vuex";
import { required, numeric } from "vuelidate/lib/validators";
export default {
  name: "FoodInputComponent",
  data() {
    return {
      input: {
        description: "",
        public: false,
        calories: 0,
        grams: 0,
        foodType: "",
      },
    };
  },
  validations: {
    input: {
      foodType: {
        required,
      },
      description: {
        required,
      },
      calories: {
        required,
      },
    },
  },
  computed: {
    ...mapGetters(["getPostVisibility", "getFoodTypes"]),
  },
  methods: {
    ...mapActions(["addInput", "addNewFoodPost"]),
    async addButtonClick() {
      if (!this.$v.input.$invalid) {
        this.addNewFoodPost(this.input);
        this.clearForm
      }
    },
    clearForm() {
      this.input.foodType=""
      this.input.description=""
      this.input.calories=0
      this.input.grams=0

    },
  },
};
</script>

<style>
.w-100 {
  width: 100%;
}
</style>