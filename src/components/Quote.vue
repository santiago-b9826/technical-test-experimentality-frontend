<template>
  <div id="quote">
      <div class="card">
        <div class="container">
            <div class="panel-defoutl">
                <p class="h1">Quotes</p>
                <p class="h2">Administrate your Quotes</p>
                <div>
                  <div v-if="selected && selected!=='Create'">
                    <input class="h5" type="text" v-model="id" :placeholder="selected"><br>
                  </div>
                  <span class="h3" v-if="message">{{ message }}</span>      
                  <form>
                    <input type="radio" id="create" value="Create" v-model="selected" @click="clearMessage">
                    <label class="h4" for="create">Create</label>
                    <input type="radio" id="obtain" value="Obtain" v-model="selected" @click="clearMessage">
                    <label class="h4" for="obtain">Obtain</label>
                    <input type="radio" id="delete" value="Delete" v-model="selected" @click="clearMessage">
                    <label class="h4" for="delete">Delete</label>
                  </form>
                  <button @click="go" class="btn btn-primary">Go</button><br> 
                </div>                           
            </div>
        </div>
    </div>
    <div class="card">
        <div class="card-div" v-if="quote.quote">
            <p>{{quote.id}}- {{quote.quote}}</p> 
            <div class="image">
              <img :src="quote.image" />
            </div>            
        </div>
        <div class="card-div" v-else>          
            <h5>{{quote.message}}</h5>
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
      id: "",
      quote: {},
      selected: "",
      input: false,
      message: ""
    };
  },
  methods: {
    clearMessage() {
      this.message = "";
    },
    go() {
      if (this.selected === "Create") {
        this.createQuote();
      } else if (this.selected === "Obtain") {
        if (this.id) {
          this.getQuote();
        } else {
          this.message = "Enter an id";
        }
      } else if (this.selected === "Delete") {
        if (this.id) {
          this.deleteQuote();
        } else {
          this.message = "Enter an id";
        }
      } else {
        this.message = "Select an option";
      }
    },
    createQuote() {
      Axios.post(`${this.protocol}${this.host}${this.path}`).then(response => {
        this.quote = response.data;
      });
    },
    getQuote() {
      Axios.get(`${this.protocol}${this.host}${this.path}/${this.id}`).then(
        response => {
          this.quote = response.data;
        }
      );
    },
    deleteQuote() {
      Axios.delete(`${this.protocol}${this.host}${this.path}/${this.id}`).then(
        response => {
          this.quote = response.data;
        }
      );
    }
  }
};
</script>

<style scoped>
.card-div {
  left: 30%;
}

.h4 {
  width: 5em;
}
.image img {
  height: 45%px;
  width: 45%;
}
.btn.btn.btn-primary{
  margin: 1em;
}
</style>
