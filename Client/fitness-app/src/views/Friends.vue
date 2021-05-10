<template>
  <div class="section">
    <div class="container">
      <div class="panel">
        <div class="panel-heading">
          <h1>
            <strong>Friends </strong>(this will be fetched from server and only
            real friends will be shown for now friends are those without +)
          </h1>
        </div>
        <div
          class="panel-block"
          v-for="user in this.getFriendProfiles"
          :key="user._id"
        >
          <PersonItem :user="user"></PersonItem>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import PersonItem from "../components/PersonItem";
import { mapGetters,mapActions } from "vuex";

export default {
  name: "Friends",
  components: {
    PersonItem,
  },
  methods:{
    ...mapActions(["fetchFriendProfiles","fetchFriendIds"])
  },
  computed: {
    ...mapGetters(["getFriendProfiles"]),
  },
  mounted(){
    this.fetchFriendProfiles()
    this.fetchFriendIds()
    console.warn(this.getFriendProfiles)
  }
};
</script>