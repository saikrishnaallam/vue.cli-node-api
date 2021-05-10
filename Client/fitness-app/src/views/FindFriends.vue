<template>
  <div class="section">
    <div class="container">
      <div class="panel is-dark">
        <div class="panel-heading">
          <div class="field has-addons is-fullwidth">
            <div class="control has-icons-left is-expanded">
              <!-- <input
                class="input"
                type="text"
                placeholder="Search for people"
                v-model.trim="search"
              />
              <span class="icon is-small is-left">
                <i class="fas fa-search"></i>
              </span> -->
              <b-autocomplete
                rounded
                v-model="name"
                :data="usernames"
                placeholder="e.g. jQuery"
                icon="magnify"
                :loading="isFetching"

                clearable
                @input="getAsyncData"
                @select="option => selected = option">
                <template #empty>No results found</template>
            </b-autocomplete>
            </div>
            <div class="control">
              <a class="button is-info" @click="searchBtnClicked()">Search</a>
            </div>
          </div>
        </div>
        <div class="panel-block" v-for="user in getUsers" :key="user._id">
          <PersonItem :user="user"></PersonItem>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import PersonItem from "../components/PersonItem";
import { mapActions, mapGetters } from "vuex";
import axios from '../axiosConfig'
export default {
  name: "FindFriends",
  components: {
    PersonItem,
  },
  data() {
    return {
      search: "",
      isFetching: false,

        usernames: [
                    // 'Angular',
                    // 'Angular 2',
                    // 'Aurelia',
                    // 'Backbone',
                    // 'Ember',
                    // 'jQuery',
                    // 'Meteor',
                    // 'Node.js',
                    // 'Polymer',
                    // 'React',
                    // 'RxJS',
                    // 'Vue.js'
                ],
                                name: '',

    };
  },

  computed: {
    ...mapGetters(["getUsers"]),
                 filteredDataArray() {
                 this.data.filter((option) => {
                    return option
                        .toString()
                        .toLowerCase()
                        .indexOf(this.name.toLowerCase()) >= 0
                })
            }

  },
  methods: {
    ...mapActions(["searchForUser", "clearSearchResults","fetchFriendIds"]),

    getAsyncData(){
            this.usernames=[]

      this.isFetching = true
      axios.get('/users/autocomplete/'+this.name).then(({data})=>{
                 
      this.usernames=[]

                        data.forEach(element => {
                          console.log(element.username)
                        this.usernames.push(element.username+" "+element.firstName+" "+ element.lastName)
                        this.isFetching = false
                        });
            
         
        }).catch(()=>{
          this.isFetching = false
        })
    },

    searchBtnClicked() {
      // alert(this.name)
      this.searchForUser(this.search);
    },
  },

  mounted(){
    this.fetchFriendIds()
  },
  beforeDestroy() {
    this.clearSearchResults();
  },
};
</script>