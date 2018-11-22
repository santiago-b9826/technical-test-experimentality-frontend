<template>
  <div id="quote">
      <div class="card">
        <div class="container">
            <div class="panel-defoutl">
                <p class="h1">Quotes</p>
                <p class="h2">Administrate your Quotes</p>

                <form name="Create-Quote-Form">
                    <div class="form-row">
                        <div class="form-group col-md-6">
                            <label class="label">Create Quote: </label>
                            <button @click="createQuote" class="btn btn-primary">Generate</button> 
                        </div>
                      </div>
                </form>  
                
                              
                <form name="Get-Quote-Form">
                    <div class="form-row">     
                        <div class="form-group col-md-6">
                            <label class="label">Obtain Quote: </label>
                            <input id='obtainInput' class="form-control" v-model="getQ">
                            <button @click="getQuote" class="btn btn-primary">Obtain</button>                            
                        </div>
                    </div>
                </form>  
                
                <form name="Delete-Quote-Form">
                    <div class="form-row">   
                        <div class="form-group col-md-6">
                            <label class="label">Delete Quote: </label>
                            <input id='deleteInput' class="form-control" v-model="deleteQ">
                            <button @click="deleteQuote" class="btn btn-primary">Delete</button> 
                        </div>
                    </div>
                </form> 
                              
            </div>
        </div>
        <div>
            <div v-if="quote.quote">          
                <h5>Id {{quote.id}}</h5>
                <p>Quote: {{quote.quote}}</p> 
                <div>
                    <td><img :src="quote.image" /></td>
                </div>
            </div>
            <div v-else>          
                <h5>{{quote.message}}</h5>
            </div>
        </div>
    </div>
  </div>
</template>

<script>
import Axios from "axios";

export default {
  name: "quote",
  data() {
    return {
      protocol: "http://",
      host: "localhost:5000",
      path: "/api/v1/generate-changing-life-quote",
      getQ: "",
      deleteQ: "",
      quote: {}
    };
  },
  methods: {
    createQuote(e) {
      e.preventDefault();
      Axios.post(`${this.protocol}${this.host}${this.path}`).then(response => {
        this.quote = response.data;
      });
    },
    getQuote(e) {
      e.preventDefault();
      if (this.getQ) {
        Axios.get(`${this.protocol}${this.host}${this.path}/${this.getQ}`).then(
          response => {
            this.quote = response.data;
          }
        );
      } else {
      }
    },
    deleteQuote(e) {
      e.preventDefault();
      if (this.deleteQ) {
        Axios.delete(
          `${this.protocol}${this.host}${this.path}/${this.deleteQ}`
        ).then(response => {
          this.quote = response.data;
        });
      }
    }
  }
};
</script>

<style>
</style>
