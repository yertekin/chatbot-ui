<template>
  <div id="app">
    <div id="nav">
      <b-navbar>
        <b-navbar-brand>
          <img src="./assets/robot.png" width="12%" height="auto">
          SU-Bot
        </b-navbar-brand>
      </b-navbar>
    </div>
    <b-container>
      <ul>
        <li v-for="message in messages" :key="message.id">
            <div class="received" v-if=message.isAgent>
              <b-card border-variant="info">
                <b-card-text>{{message.payload}}</b-card-text>
              </b-card>
            </div>
            <div class="sent" v-else>
              <b-card border-variant="info">
                <b-card-text>{{message.payload}}</b-card-text>
              </b-card></div>
        </li>
      </ul>
    </b-container>
    <footer id="send-message">
      <b-row>
        <b-col cols="9">
          <b-form-input v-model="text" placeholder="Enter your message"></b-form-input>
        </b-col>
        <b-col cols="2">
          <b-button variant="outline-primary">
            <b-icon icon="arrow-right-circle-fill"></b-icon>
          </b-button>
        </b-col>
      </b-row>
    </footer>
  </div>
</template>

<script>
import axios from 'axios'


export default {
  name: 'App',
  mounted() {
    axios.get('http://localhost:3000/content').then(
      (response) => {
        for(let key in response.data) {
          this.messages.push(response.data[key])
        }
    });
  },
  data() {
      return {
        text: '',
        messages: []
      }
  }    
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  width: 100%;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}
#nav {
  background-color: #124b8f;
}
.navbar.navbar-expand {
    justify-content: center;
}
.navbar.navbar-light .navbar-brand {
    color: white;
}
.container {
  margin-top: 15px;
  scroll-margin-bottom: 15px;
  flex: auto;
  overflow-y: scroll;
}
#send-message {
  bottom: 0%;
  margin-bottom: 10px;
}
html {
  height: 100%;
}
.row {
  justify-content: center;
}
ul {
    list-style-type: none;
}
li {
  padding-bottom: 10px;
  
}
.sent {
  text-align: right;
  width: 50%;
  margin: 0 auto;
}

.received {
  text-align: left;
  margin: 0 auto;
  width: 50%;
}
footer {
  padding-left: 15px;
}
</style>
