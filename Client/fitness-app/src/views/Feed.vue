<template>
  <div class="panel">
    <div class="panel-heading">
      <div class="level">
        <div class="level-left">
          <span>Feed</span>
        </div>
        <div class="level-right">
          <div class="select is-small">
            <select v-model="selectedFilter" @change="onChange($event)">
              <option
                v-for="filter in feedFilterOptions"
                :value="filter.name"
                :key="filter.name"
              >
                {{ filter.name }}
              </option>
            </select>
          </div>
        </div>
      </div>
    </div>

    <Post v-for="post in getAllPosts" :key="post.id" :post="post" />
  </div>
</template>

<script>
import { mapGetters, mapActions } from "vuex";
import Post from "../components/Post";

export default {
  name: "Feed",
  components: { Post },

  computed: {
    ...mapGetters(["getFeedFilterOptions", "isLoggedIn", "getAllPosts"]),

    feedFilterOptions() {
      //shows only available filter types
      return this.getFeedFilterOptions.filter(
        (option) =>
          !option.needsToBeLoggedIn ||
          (option.needsToBeLoggedIn && this.isLoggedIn)
      );
    },
  },
  methods: {
    ...mapActions(["fetchPublicPosts","fetchMyPosts","fetchFriendPosts"]),
    onChange(event) {
      switch (event.target.value) {
        case 'My posts':
           this.fetchMyPosts();
          break;
        case 'Public posts':
           this.fetchPublicPosts();
          break;
        case 'Friends posts':
          this.fetchFriendPosts();

        default:
        // code block
      }
    },
  },
  mounted() {
    this.fetchPublicPosts();
  },

  data() {
    return {
      selectedFilter: "Public posts",
    };
  },
};
</script>