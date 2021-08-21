<template>

    <div class="home">
      <div v-if="projects.length">
          <div v-for="project in projects" :key="project.id">
                <div>
                    <single-project :project="project" @complete="handleComplete" @delete="handleDelete"/>
                </div>
          </div>
      </div>
  </div>
</template>

<script>
// @ is an alias to /src

import SingleProject from "../components/SingleProject";
import BASE_URL from "../utility/constants";
import NavBar from "../components/Navbar";
export default {
  name: 'Home',
  components: {
      NavBar,
      SingleProject

  },
    data() {
      return {
          projects: []
      }
    },
    mounted() {
      fetch(BASE_URL)
          .then(res => res.json())
          .then( data => this.projects = data)
          .catch(err => console.log(err.message))
    },
    methods: {
      handleDelete(id) {
          this.projects = this.projects.filter((item=> item.id !== id))
      },
        handleComplete(id) {
          let toBeCompleted = this.projects.find( item => item.id === id );
          toBeCompleted.complete = !toBeCompleted.complete;
        }
    }
}
</script>
