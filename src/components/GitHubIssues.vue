<template>
    <div class="container">
        
        <form action="" class="form">
            <div class="square">
                <div class="inputs">
                    <input v-model="username" type="text" id="" placeholder="github username">
                    <input v-model="repository" type="text" id="" placeholder="github repository">
                </div>

                <div class="buttons">
                    <button @click.prevent.stop="getIssues()" >Search</button>
                    <button @click.prevent.stop="reset()" >Clear</button>
                </div>
            </div>    
        </form>   

        <table class="list-table">
            <thead>
                <tr>
                    <th width="100">Número</th>
                    <th>Título</th>
                </tr>
            </thead>

            <tbody>

                <tr v-if="loader.getIssues">
                    <td colspan="2"><img src="/static/load.svg"></td>
                </tr>

                <tr v-if="!!issues.length && !loader.getIssues"
                    v-for="issue in issues" 
                    :key="issue.number">
                    <td>{{ issue.number }}</td>
                    <td>{{ issue.title }}</td>
                </tr>

                <tr v-if="!!!issues.length && loader.getIssues">
                    <td class="text-center" colspan="2">Nenhuma issues encontrada!</td>
                </tr>
            </tbody>
        </table>
               
    </div>
</template>

<script>

import axios from 'axios';

export default {
  name: 'GitHubIssues',

  data(){
      return{
          username: '',
          repository: '',
          issues: [],
          loader: {
              getIssues: false,
          },
      };
  },

  methods: {
      reset(){
          this.username = '';
          this.repository = '';
      },

      getIssues(){
            if(this.username && this.repository){
               this.loader.getIssues = true;
               const url = 'https://api.github.com/repos/' + this.username + '/' + this.repository   + '/issues';
               axios.get(url).then( (res) => {
                 this.issues = res.data;
                 console.log(res);
            }).finally( () => {
                this.loader.getIssues =  false;
            });
        }    
      },
  },

}
</script>
