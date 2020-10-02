<template>
  <div id="app">
    <div class="header">
      <h1 class="title">Alexander Ege's Chatroom</h1>
      <p class="username">Username: {{ username }}</p>
      <p class="online">{{ users.length }} Users Online: [<span v-for="user in users" :key=user.username>
        <span v-if="users.indexOf(user) == (users.length - 1)">{{user}}</span>
        <span v-else >{{user}},</span>
      </span>]</p>
    </div>
    <ChatRoom v-bind:messages="messages" v-on:sendMessage="this.sendMessage" :username="username"/>
  </div>
</template>

<script>

import io from 'socket.io-client';
import ChatRoom from './components/ChatRoom';

export default {
  name: 'App',
  
  components: {
    ChatRoom
  },
  
  data: function() {
    return {
      username: "",
      socket:io("http://localhost:3000"),
      messages: [],
      users: []
    }
  },
  
  methods: {
    joinServer: function() {
      this.socket.on("loggedIn", data => {
        this.messages = data.messages;
        this.users = data.users;
        this.socket.emit('newuser', this.username);
      });

      this.listen();
    },
    listen: function() {
     
      // Add user to users array
      this.socket.on("userOnline", user => {
        this.users.push(user);
      });

      // Remove user from users array
      this.socket.on('userLeft', user => {
        this.users.splice(this.users.indexOf(user), 1);
      });

      // Add message to messages array
      this.socket.on('msg', message => {
        this.messages.push(message);
      });
    },
    sendMessage: function (message) {
      this.socket.emit('msg', message);
    }
  },

  mounted: function() {
    this.username = prompt("What is your username?", "Anonymous");

    // If no username provided, assign username to Anonymous
    if(!this.username) {
      this.username = "Anonymous";
    }

    this.joinServer();
  }
}
</script>

<style lang="scss">
body {
  font-family: 'avenir', Helvetica, Arial, sans-serif;
  background-color: black;
  color: white;
  margin: 0;
  padding: 0;
}

#app {
  display: flex;
  flex-direction: column;
  height: 100vh;
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 15px;
  box-sizing: border-box;
}

.title {
  text-align: center;
}
</style>
