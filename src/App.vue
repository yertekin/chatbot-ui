<template>
  <div id="app">
    <div id="nav">
      <b-navbar>
        <b-navbar-brand>
          Sabanci University Assistant
        </b-navbar-brand>
      </b-navbar>
    </div>
    <b-container fluid id="messageContainer">
      <subot-message-container
        :messageList="messages"
      ></subot-message-container>
    </b-container>
    <footer id="send-message">
      <b-row>
        <b-col cols="10">
          <b-form-input
            v-model="text"
            placeholder="Enter your message"
            id="messageInput"
          >
          </b-form-input>
        </b-col>
        <b-col cols="2">
          <b-button
            variant="outline-primary"
            v-on:click="
              addQuestion();
              askQuestion();
              removeText();
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
      let response = await axios.post("http://localhost:8080/create");
      this.sessionId = response.data.id;
      localStorage.setItem("sessionId", this.sessionId);
    } else {
      this.sessionId = localStorage.getItem("sessionId");
    }
  },
  methods: {
    updateScroll() {
      var element = document.getElementById("messageContainer");
      element.scrollTop = element.scrollHeight;
    },
    async askQuestion() {
      if (this.text !== "" && this.text !== null) {
        const question = { sessionId: this.sessionId, message: this.text };
        const response = await axios.post(
          "http://localhost:8080/ask",
          question
        );
        this.pushMessage(response.data);
        this.updateScroll();
      }
    },
    addQuestion() {
      if (this.text !== "" && this.text !== null) {
        let question = { message: this.text, agent: false };
        this.pushMessage(question);
      } else {
        this.pushMessage({ message: "Please enter a question", agent: true });
      }
      this.updateScroll();
    },
    removeText() {
      let target = document.getElementById("messageInput");
      target.value = "";
      this.text = "";
    },
    pushMessage(message) {
      this.messages.push(message);
      this.updateScroll();
    }
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
  color: #2c3e50;
  width: 100%;
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}
#nav {
  position: fixed;
  width: 100vw;
  text-align: center;
  top: 0;
  background-color: #124b8f;
  z-index: 999999;
}
.navbar.navbar-expand {
  justify-content: center;
}
.navbar.navbar-light .navbar-brand {
  color: white;
}
#messageContainer {
  padding-top: 71px;
  padding-bottom: 53px;
  flex: auto;
  overflow: auto;
  background-color: #F5F5F5;
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
ol{
  padding-left: 0;
}
ul {
  list-style-type: none;
}
li {
  padding-bottom: 10px;
}
footer {
  position: fixed;
  bottom: 0;
  padding-left: 12px;
  width: 100vw;
}
.row > .col-10 {
  padding-right: 0;
}
.row > .col-2 {
  padding-left: 0;
}
</style>
