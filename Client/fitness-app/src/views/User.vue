<template>
  <div class="panel">
    <div class="panel-heading">
      <div class="level">
        <div class="level-left">
          <span>@{{ user.username }}</span>
          <button
            v-if="isUserFriend"
            class="addFbtn button is-info is-small"
            @click="follow"
          >
            Follow
          </button>
          <button
            v-if="!isUserFriend"
            class="addFbtn button is-danger is-small"
            @click="unFollow"
          >
            Unfollow
          </button>
        </div>
      </div>
    </div>
    <div class="box">
      <div class="level-left">
        <p class="overview">
          <strong>Username:</strong>
          {{ user.username }}
        </p>
      </div>

      <div class="level-left">
        <p class="overview">
          <strong>First name:</strong>
          {{ user.firstName }}
        </p>
      </div>
      <div class="level-left">
        <p class="overview">
          <strong>Last name:</strong>
          {{ user.lastName }}
        </p>
      </div>
    </div>
     <div class="panel-heading">
      <div class="level">
        <div class="level-left">
          <span>{{user.firstName+' '+user.lastName }} history</span>
        </div>
      
      </div>
    </div>

    <Post
      v-for="post in postsOfUser"
      :key="post._id"
      :post="post"
    />
  </div>
</template>

<script>
import { mapGetters, mapActions } from "vuex";
import Post from "../components/Post";
import axios from "../axiosConfig";
export default {
  name: "User",
  components: { Post },
  props: {
    id: {
      type: String,
      required: true,
    },
  },
  computed: {
    ...mapGetters([, "isFriend", "getCurrentUser", "getFriendIds"]),
    isUserFriend: function () {
      return !this.getFriendIds.includes(this.user._id);
    },
  },
  data() {
    return {
      user: {
        username: "",
        firstName: "",
        lastName: "",
      },
      areFriends: false,
      postsOfUser:[],
    };
  },
  async mounted() {
    console.log(this.id);
    await this.fetchFriendIds();

    await axios.get("/users/profile/" + this.id).then((data) => {
      this.user = data.data;
    });
    axios.get('/posts/friend-posts/'+this.id).then(data=>this.postsOfUser=data.data)
  },
  methods: {
    ...mapActions(["removeFrined", "addFriend", "fetchFriendIds","addFriendToList"]),

     follow() {
        axios.patch("users/follow/"+this.user._id).then(data=>{
              this.addFriendToList(this.user._id)

        })
    },
     unFollow() {
        axios.patch("users/unfollow/"+this.user._id).then(data=>{
          console.log(data)
          this.removeFrined(this.user._id)
        })
    }
  },
};
</script>

<style scoped>
p {
  font-size: 1.2rem;
}
.addFbtn {
  margin-left: 10px;
}
</style>