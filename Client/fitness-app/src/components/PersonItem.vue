<template>
  <div class="card w-100 person">
    <div class="card-content">
      <div class="media">
        <div class="media-left">
          <figure class="image is-64x64">
            <img
            class="rounded-circle"
            style="border-radius:50%"
              :src=url
              alt="not loaded"
            />
          </figure>
        </div>
        <div class="media-content">
          <p class="title is-4">
            <router-link :to="`/users/${user.username}`"
              >{{ user.firstName }} {{ user.lastName }}</router-link
            >
          </p>
          <p class="subtitle is-6">
            <router-link :to="`/users/${user.username}`"
              >@{{ user.username }}</router-link
            >
          </p>
        </div>
        <div class="media-right">
          <button
            class="button is-primary"
            v-if="this.isUserFriend" 
            @click="follow()"
          > 
          <strong>Follow</strong>
            <span class="icon">
              <i class="fas fa-plus"></i>
            </span>
          </button>
        </div>
         <div class="media-right">
          <button
            class="button is-danger"
            v-if="!this.isUserFriend" 
            @click="unFollow()"
          > 
          <strong>Unfollow</strong>
            <span class="icon">
              <i class="fas fa-plus"></i>
            </span>
          </button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from "vuex";
import axios from '../axiosConfig'

export default {
  name: "PersonItem",
  props: {
    user: {
      type: Object,
      required: true,
      default: null
    },
    addFriendVisible: {
      type: Boolean,
      default: true
    }
  },
  data() {
    return {
      url:''
    };
  },
  computed:{
    ...mapGetters([
      "getCurrentUserId",
      'isFriend',
      "isLoggedIn",
      "getFriendIds",
    ]),
   isUserFriend : function () {
     console.log(this.user._id)
     console.log(this.getFriendIds)
      return !this.getFriendIds.includes(this.user._id)
    },

  },
  methods:{
     ...mapActions(['removeFrined','addFriendToList']),
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
  mounted() {
    if(this.user.imageUrl!=''){
        this.url='http://localhost:3000/'+this.user.imageUrl
    }else{
      this.url='https://www.attendit.net/images/easyblog_shared/July_2018/7-4-18/totw_network_profile_400.jpg'
    }

  },
};
</script>

<style scoped>
.person {
  border-top: 1px rgb(220, 220, 220) solid;
}
.rounded-circle{
border-radius:50%;
width:50px;
height:50px;
}
</style>