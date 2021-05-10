<template>
  <div class="columns is-centered">
    <div class="column is-half">
      <div class="section">
        <div class="panel is-primary">
                  <p class="panel-heading">Usage statistics</p>

          <div class="panel-block">
            <div class="stField field">
              <span class=" icon is-small is-left is-inline">
                <i class="fas fa-user"></i>
              </span>
                <label class="label is-inline">Number of users: {{statistics.users}}</label>
            </div>
          </div>
        
          <div class="panel-block">
            <div class="stField field">
              <span class="icon is-small is-left is-inline">
                <i class="fas fa-dumbbell"></i>
              </span>
                <label class="label is-inline">Number of exercise posts: {{statistics.foodPosts}}</label>
            </div>
          </div>
          <div class="panel-block">
            <div class="stField field">
              <span class="icon is-small is-left is-inline">
                <i class="fas fa-hamburger"></i>
              </span>
                <label class="label is-inline">Number of food posts: {{statistics.exercisePosts}}</label>
            </div>
          </div>
            <div class="panel-block">
            <div class="stField field">
              <span class="icon is-small is-left is-inline">
                <i class="fas fa-tally"></i>
              </span>
                <label class="label is-inline">Number of total posts: {{statistics.foodPosts+statistics.exercisePosts}}</label>
            </div>
          </div>
            <div class="panel-block">
            <div class="stField field">
              <span class="icon is-small is-left is-inline">
                <i class="fas fa-fire-alt"></i>
              </span>
                <label class="label is-inline">Total calories burned:{{statistics.burnedCalories }}</label>
                
            </div>
            
          </div>
            <div class="panel-block">
            <div class="stField field">
              <span class="icon is-small is-left is-inline">
                <i class="fas fa-hamburger"></i>
              </span>
                <label class="label is-inline">Total  calories ingested:{{statistics.ingedstedCalories}}</label>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from '../axiosConfig';
import { mapGetters } from "vuex";

export default {
  name: "Statistics",
  data(){
    return{
      statistics:{
        foodPosts: 0,
    exercisePosts: 0,
    users: 0,
    ingedstedCalories: 0,
    burnedCalories: 0
      }
    }
  },
  computed:{
    ...mapGetters(["getCurrentUser"]),

  },
  mounted() {
    if(this.getCurrentUser.role==='ADMIN'){
      axios.get('posts/statistics').then(data=>this.statistics=data.data)
    }else{
      this.$router.push({ path: '/' })

    }
  },
};
</script>

<style scoped>
.stField{
    padding: 10px;
}
.label{
    margin-left: 10px;
}
</style>