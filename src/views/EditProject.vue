<template>
    <h1>Edit Project {{id}}</h1>
    <form @submit.prevent="handleSubmit">
        <label for="title">Title: </label>
        <input id="title" v-model="title" type="text" required>

        <label for="details">Details: </label>
        <textarea spellcheck="false" id="details" v-model="details" required>

        </textarea>

        <button type="submit">Update Project</button>
    </form>
</template>

<script>
    import BASE_URL from "../utility/constants";
    export default {
        name: "editProject",
        props: ['id'],
        data() {
            return {
                title: '',
                details: '',
            }
        },

        mounted() {
            fetch(`${BASE_URL}${this.id}`).then(res => res.json()).then(data => {

               this.title = data.title;
                this.details = data.details;

            })
        },
        methods: {
            handleSubmit() {
                let editProject = {title: this.title, details: this.details, complete: this.complete }

                fetch(`${BASE_URL}${this.id}`, {
                    method: 'PATCH',
                    headers: {'Content-Type': 'application/json'},
                    body: JSON.stringify(editProject)
                }).then(() => {
                    this.title = '';
                    this.details = '';
                    this.$router.push('/')
                }).catch(err => console.log(err.message))
            }
        }
    }
</script>

<style scoped>
    form {
        background: white;
        padding: 20px;
        border-radius: 10px;
    }
    label {
        display: block;
        color: #bbb;
        text-transform: uppercase;
        font-size: 14px;
        font-weight: bold;
        letter-spacing: 1px;
        margin: 20px 0 10px 0
    }
    input {
        padding: 10px;
        border: 0;
        border-bottom: 1px solid #ddd;
        width: 100%;
        box-sizing: border-box;
    }
    textarea {
        border: 1px solid #ddd;
        padding: 10px;
        width: 100%;
        box-sizing: border-box;
        height: 100px;
    }
    form button {
        display: block;
        margin: 20px auto 0;
        background: #00ce89;
        color: white;
        padding: 10px;
        border: 0;
        border-radius: 6px;
        font-size: 16px;
    }

    form button:hover {

    }
</style>