<template>
  <div class="panel">
    <div class="panel-heading">
      <div class="level">
        <div class="level-left">
          <span>Dashboard</span>
        </div>
      </div>
    </div>
    <div class="box level">
      <div class="level-left">
        <i class="fas fa-balance-scale"></i>
        <p class="overview"><strong>Weight:</strong> {{this.getCurrentUserWeight}}kg</p>
      </div>
      <div class="level-left">
        <i class="fas fa-hamburger"></i>
        <p class="overview"><strong>Calories ingested today:</strong> {{ingestedCalories}}</p>
      </div>
      <div class="level-left">
        <i class="fas fa-dumbbell"></i>
        <p class="overview"><strong>Calories burned today:</strong> {{burnedCalories}}</p>
      </div>
    </div>
    <div v-if="isLoggedIn" class="column"><FoodAndExerciseInput /></div>

    <div class="panel-heading">
      <div class="level">
        <div class="level-left">
          <span>My history</span>
        </div>

      </div>
    </div>

    <Post
      v-for="post in getAllPosts"
      :key="post._id"
      :post="post"
    />
  </div>
</template>

<script>
import { mapGetters, mapActions } from "vuex";
import Post from "../components/Post";
import FoodAndExerciseInput from "../components/FoodAndExerciseInput";
import axios from '../axiosConfig'

export default {
  name: "Dashboard",
  components: { Post, FoodAndExerciseInput },
  data() {
    return {
      selectedFilter: "My posts",
      ingestedCalories:0,
      burnedCalories:0,
      weight:0,
    };
  },
  computed: {
    ...mapGetters(["isLoggedIn","getAllPosts",'getCurrentUserWeight']),
  },
  methods: {
    ...mapActions(["fetchMyPosts"]),
  },

  mounted() {
    this.fetchMyPosts();
    axios.get('/posts/countCalories').then(data=>{
      console.warn(data)
      this.burnedCalories=data.data.burned
      this.ingestedCalories=data.data.ingested
    })
  },
};
</script>

<style scoped>
.overview {
  margin-left: 5px;
}
#exercise {
  margin-bottom: 10px;
}
</style>