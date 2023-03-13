<template>
  <GreetView msg="Welcome to Github Search"/>
  <div id="app" class="container">
    <form
      v-on:submit.prevent="getRepositories()"
      autocomplete="off"
      class="user-input"
    >
      <input
        type="text"
        v-model="username"
        name="username"
        id="username"
        placeholder="username"
      />
      <input type="submit" />
    </form>
    <div v-if="search">
      <div v-if="userexist">
        <h3>
          Showing
          <a v-bind:href="'https://github.com/' + user" target="_blank">{{
            user
          }}</a>
          repositories:
        </h3>
        {{ user }} 
        <img :src="img" width="50" height="50"/>
        {{ name }}
        <GridView v-bind:header="header" v-bind:content="repositories"></GridView>
      </div>
      <div v-else>
        <h3>User {{ user }} does not exist</h3>
      </div>
    </div>
    <div v-else>
      <h3>Enter a username</h3>
    </div>

    <div class="page-controls">
      <button @click="prevPage()" v-if="this.page !== 1">
        <i class="fas fa-chevron-left"></i>
        Previous Page
      </button>
      <button @click="nextPage()" v-if="this.numitems === 8">
        Next Page
        <i class="fas fa-chevron-right"></i>
      </button>
    </div>
  </div>
</template>

<script>
import GreetView from './components/GreetView.vue'
import GridView from "./components/GridView.vue"

export default {
  name: 'App',
  data: function () {
    return {
      header: [
        "Name",
        'Stars <i class="fas fa-star"></i>',
        'Forks <i class="fas fa-code-branch"></i>',
        'Issues <i class="fas fa-exclamation-triangle"></i>',
        'Pushed Date'
      ],
      username: "",
      user: "",
      img:"",
      repositories: [],
      userexist: false,
      search: false,
      page: 1,
      numitems: 0,
    };
  },
  methods: {
    getRepositories: async function () {
      this.userImg=`https://avatars.githubusercontent.com/u/${this.id}?v=4`;
      this.img = this.userImg;

      this.username = this.username.toLowerCase();
      const url = `https://api.github.com/users/${this.username}/repos?page=${this.page}&per_page=8`;
      const response = await fetch(url);
      this.user = this.username;
      const content = await response.json();
      if (content.message === "Not Found") {
        this.userexist = false;
      } else {
        this.userexist = true;
        this.repositories = content.map((repo) => [
          `<a href="${repo.html_url}" target="_blank">${repo.name}</a>`,
          repo.stargazers_count,
          repo.forks,
          repo.open_issues_count,
          repo.pushed_at,
        ]);
        this.numitems = this.repositories.length;
      }
      this.search = true;
    },
   
    nextPage: function () {
      this.page++;
      this.getRepositories();
    },
    prevPage: function () {
      this.page--;
      this.getRepositories();
    },
  },
  components: {
    GreetView,
    GridView
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
html,
body {
  font-family: Helvetica, sans-serif;
  background-color: #242f3a;
}

form.user-input {
  padding: 5px;
  display: flex;
}

form.user-input > input[type="text"] {
  flex-grow: 1;
}

h3 {
  text-align: center;
}

.container {
  max-width: 780px;
  margin: 20px auto;
  background-color: #f2f2f2;
  border-radius: 2px;
  padding: 2px;
}

.page-controls {
  margin: 10px auto 0px;
  width: max-content;
}

.page-controls > button {
  margin: 4px;
}

</style>



