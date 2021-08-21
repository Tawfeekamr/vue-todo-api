<template>
   <div class="project" :class="{complete: project.complete}">
      <div class="actions">
          <h3  @click="showDetails = !showDetails">{{project.title}}</h3>
          <div class="icons">
            <span @click="toggleComplete" class="material-icons tick">
            done
            </span>
                <router-link :to="{name: 'EditProject', params: {id: project.id}}">
                    <span @click="editProject" class="material-icons">
                        edit
                    </span>
                </router-link>
              <span @click="deleteProject" class="material-icons">
            delete
            </span>
          </div>
      </div>
       <div class="details" v-if="showDetails">
           <p>
               {{project.details}}
           </p>
       </div>
   </div>
</template>

<script>
    import BASE_URL from "../utility/constants";

    export default {
        name: "SingleProject",
        data() {
            return {
                showDetails: false,
                uri: `${BASE_URL}${this.project.id}`,
                complete: false
            }
        },
        props: [
            'project'
        ],
        methods: {
            deleteProject() {
                fetch(this.uri, { method: 'DELETE' } )
                .then(() => this.$emit('delete', this.project.id))
                .catch(err => console.log(err.message))
            },
            toggleComplete() {
                fetch(this.uri, {
                    method: 'PATCH',
                    headers: {'Content-Type': 'application/json'},
                    body: JSON.stringify({complete: !this.project.complete})
                })
                .then(() => this.$emit('complete', this.project.id))
                .catch(err => console.log(err.message))
            },
            editProject() {

            }
        }
    }
</script>

<style scoped>
    .project {
        margin: 20px auto;
        background: white;
        padding: 10px 20px;
        border-radius: 4px;
        box-shadow: 1px 2px 3px rgba(0,0,0,0.05);
        border-left: 4px solid #e90074;
    }
    h3 {
        cursor: pointer;
    }
    .actions {
        display: flex;
        justify-content: space-between;
        align-items: center;
    }
    .material-icons {
        font-size: 24px;
        margin-left: 10px;
        color: #bbb;
        cursor: pointer;
    }
    .material-icons:hover {
        color: #777;
    }
    /* completed projects */
    .project.complete {
        border-left: 4px solid #00ce89;
    }
    .project.complete .tick {
        color: #00ce89;
    }
</style>