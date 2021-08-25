<template>
  <div id="app">
    <div id="nav">
      <b-navbar>
        <b-navbar-brand>
          <img src="./assets/robot.png" width="12%" height="auto" />
          SU-Bot
        </b-navbar-brand>
      </b-navbar>
    </div>
    <b-container>
      <subot-message-container
        :messageList="messages"
      ></subot-message-container>
    </b-container>
    <footer id="send-message">
      <b-row>
        <b-col cols="9">
          <b-form-input v-model="text" placeholder="Enter your message">
          </b-form-input>
        </b-col>
        <b-col cols="2">
          <b-button
            variant="outline-primary"
            v-on:click="
              addQuestion();
              askQuestion();
            "
          >
            <b-icon icon="arrow-right-circle-fill"></b-icon>
          </b-button>
        </b-col>
      </b-row>
    </footer>
  </div>
</template>

<script>
import axios from "axios";
import MessagesContainer from "./components/MessagesContainer.vue";

export default {
  name: "App",
  components: {
    "subot-message-container": MessagesContainer,
  },
  async created() {
    if (localStorage.getItem("sessionId") === null) {
      let response = await axios.post(
        "http://localhost:8080/create", {ping: "pong"}
      );
      this.sessionId = response.data.id;
      localStorage.setItem("sessionId", this.sessionId);
    } else {
      this.sessionId = localStorage.getItem("sessionId");
    }
  },
  methods: {
    async askQuestion() {
      const question = { sessionId: this.sessionId, message: this.text };
      const response = await axios.post(
        "http://localhost:8080/ask",
        question
      );
      for (let key in response.data) {
        this.messages.push(response.data[key]);
      }
    },
    addQuestion() {
      let question = { message: this.text, isAgent: false };
      this.messages.push(question);
      this.$forceUpdate();
      console.log(this.messages);
    },
  },
  data() {
    return {
      text: "",
      messages: [],
      sessionId: 0,
    };
  },
};
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
  margin: 0 auto;
}

.received {
  text-align: left;
  margin: 0 auto;
}
footer {
  padding-left: 15px;
}
</style>
