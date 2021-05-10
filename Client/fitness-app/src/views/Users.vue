<template>
  <div class="container">
    <div class="box">
      <div class="table-container">
        <table class="table is-fullwidth">
          <thead>
            <tr>
              <th>
                <div id="profileImage" class="media-left level-left">
                  <figure class="image is-48x48">
                    <img
                    
                      src="https://www.attendit.net/images/easyblog_shared/July_2018/7-4-18/totw_network_profile_400.jpg"
                      alt="Image"
                    />
                  </figure>
                  <p id="uN">Username</p>
                </div>
              </th>
              <th>Name</th>
              <th>Surname</th>
              <th>Email</th>
              <th>Delete</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="user in users" :key="user._id">
              <th>
                <div id="profileImage" class="media-left level-left">
                  <figure class="image is-48x48">
                    <img
                      class="rounded-circle"
                      :src= imageUrl(user)
                      alt="Image"
                    />
                  </figure>
                  <p id="uN">
                    <router-link :to="'/users/' + user._id"
                      >{{ user.username }}
                    </router-link>
                  </p>
                </div>
              </th>
              <th>{{ user.firstName }}</th>
              <th>{{ user.lastName }}</th>
              <th>{{ user.email }}</th>
              <th>
                <button
                  class="button is-danger"
                  @click="deleteUserClick(user._id)"
                >
                  Delete
                </button>
              </th>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "../axiosConfig";
import { mapGetters } from "vuex";

export default {
  name: "Users",
  data() {
    return {
      users: [],
    };
  },
  mounted() {
    if(this.getCurrentUser.role!='ADMIN'){
            this.$router.push({ path: '/' })

    }else{
    axios.get("/users/all").then((data) => {
      this.users = data.data;
    });
  }
  },
  computed: {
    ...mapGetters(["getAllUsers","getCurrentUser"]),
      
  },
  methods: {
     imageUrl (user) {
         console.log(user.imageUrl)
         var url='http://localhost:3000/'
         if(user.imageUrl!=''){
           return url+user.imageUrl
         }else{
           return 'https://www.attendit.net/images/easyblog_shared/July_2018/7-4-18/totw_network_profile_400.jpg'
         }
       },
    deleteUserClick(id) {
      var answer = window.confirm("Delete user ?");
      if (answer) {
        axios.delete("/users/" + id).then(() => {
          alert("User has been deleted");

          var index = this.users.findIndex((user) => user._id === id);
          this.users.splice(index, 1);
        });
      } else {
        //some code
      }
    },
  },
};
</script>

<style scoped>
#uN {
  margin-left: 10px;
}
.rounded-circle{
border-radius:50%;
width:50px;
height:50px;
}
</style>