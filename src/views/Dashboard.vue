<template>
  <div class="container">
    <h1>Dashboard</h1>
    <hr>
    <h3>Create a Text Snippet for Analyze</h3>

    <div id="snippet-create" class="container">
      <div class="form-inputs">
        <textarea class="form-control form" id="snippet" name="snippet" v-model="input.snippet" placeholder="Snippet" />
      </div>
      <br>
      <button type="button" v-on:click="addSnippet()">Add Snippet</button>
    </div>
    <hr>

    <div v-for="(snippet, index ) in snippets" :key="index" class="card mb-3"
      style="margin-bottom: 50px; border: 2px dotted grey; padding: 15px; border-radius: 10px;">
      <div class="card-body" style="margin-bottom: 20px;">
        <h4 class="card-title">Snippet {{ snippet.id }}</h4>
        <div class="card-text">{{ snippet.snippet }}</div>
      </div>
      <button class="btn btn-success" v-on:click="getAnalysis(index, snippet)">Get analysis</button>

      <ul v-if="snippet.analysis" class="list-group" style="margin-top: 10px;">
        <li class="list-group-item d-flex justify-content-between align-items-center">
          Character count
          <span class="badge bg-primary rounded-pill">{{ snippet.analysis.count_characters }}</span>
        </li>
        <li class="list-group-item d-flex justify-content-between align-items-center">
          Paragraph count
          <span class="badge bg-primary rounded-pill">{{ snippet.analysis.count_paragraphs }}</span>
        </li>
        <li class="list-group-item d-flex justify-content-between align-items-center">
          Sentence count
          <span class="badge bg-primary rounded-pill">{{ snippet.analysis.count_sentences }}</span>
        </li>
        <li class="list-group-item d-flex justify-content-between align-items-center">
          Word Count
          <span class="badge bg-primary rounded-pill">{{ snippet.analysis.count_words }}</span>
        </li>
        <li class="list-group-item d-flex justify-content-between align-items-center">
          Longest words in paragraphs
          <span class="badge bg-primary rounded-pill">{{ snippet.analysis.longest_word_in_paragraphs }}</span>
        </li>
      </ul>
      <br>
    </div>
  </div>
</template>


<script>
    import axios from 'axios'
    export default {
        name: 'Dashboard',
        data() {
          return {
            basePath: 'http://localhost:3313/api',
            snippets: [],
            input: {
              snippet: "",
            }
          };
        },
        mounted() {
            if(!this.$parent.authenticated) {
                this.$router.replace({ name: "Login" });
            }
          this.loadSnippets();
        },
        methods: {
          getHeaders() {
            const authData = JSON.parse(localStorage.getItem('authData'));
            const headers = {
              'Content-Type': 'application/json',
              'Authorization': 'Bearer ' + authData.accessToken
            }

            return {headers};
          },
          async loadSnippets() {
            await axios.get(this.basePath + '/snippets', this.getHeaders()).then(response => {
              this.snippets = response.data.data.map(snippet => {
                snippet.analysis = null;
                return snippet;
              });
            }).catch(error => {
              alert(error.response.data.message);
            });
          },
          async getAnalysis(index, snippet) {
            const payload = { 
              snippet: snippet.snippet
            };
            await axios.post(this.basePath + '/snippets/analyze-all',  payload, this.getHeaders()).then(response => {
              this.snippets[index].analysis = response.data.data;
            }).catch(error => {
              alert(error.response.data.message);
            });
          },
          async addSnippet(snippet) {
            const payload = { 
              snippet: this.input.snippet
            };
            await axios.post(this.basePath + '/snippets',  payload, this.getHeaders()).then(response => {
              const result = response.data.data;
              result.analysis = null;
              this.snippets.push(result);
            }).catch(error => {
              alert(error.response.data.message);
            });
          }
        }
    }
</script>