<template>

    <div class="home">
        <filter-nav @filterChanged="current = $event" :current="current" />
      <div v-if="projects.length">
          <div v-for="project in filterProjects" :key="project.id">
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
import FilterNav from "../components/FilterNav";
export default {
  name: 'Home',
  components: {
      FilterNav,
      NavBar,
      SingleProject

  },
    data() {
      return {
          projects: [],
          current: 'all'
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
        },

    },
    computed: {
        filterProjects() {
            switch (this.current) {
                case 'all': {
                    return this.projects
                }
                case 'completed': {
                    return this.projects.filter(item => item.complete === true)
                }
                case 'not-completed': {
                   return  this.projects.filter(item => item.complete === false)
                }
                default: {
                    return this.projects
                }
            }
        }
    }
}
</script>
