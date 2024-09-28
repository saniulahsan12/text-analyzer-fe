<template>
  <div class="secure">
    <h1>This is an secure page</h1>
    <button v-on:click="loadSnippets()">Load Text Snippets</button>
  </div>
</template>


<script>
    import axios from 'axios'
    export default {
        name: 'Secure',
        data() {
          return {}
        },
        mounted() {
            if(!this.$parent.authenticated) {
                this.$router.replace({ name: "Login" });
            }
        },
        methods: {
          async loadSnippets() {
            const authData = JSON.parse(localStorage.getItem('authData'));
            console.log(authData);
            await axios.get('http://localhost:3313/api/snippets').then(response => {
              console.log(response.data.data);
            }).catch(error => {
              alert(error.response.data.message);
            });
          }
        }
    }
</script>