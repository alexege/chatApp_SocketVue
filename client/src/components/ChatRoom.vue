<template>
    <div class="chat-window">
        <div class="messages">
            <div class="message" v-for="message in messages" v-bind:key="message.index">
                <span :class="username == message.username ? 'currentUser' : 'username'">{{ message.username }}:</span><span class="mssage-text">{{message.msg}}</span>
                
                <a href="/deleteMessage/" v-if="username == message.username" class="deleteIcon"><i class="fas fa-trash-alt"></i></a>
            </div>
        </div>
        <form class="input-container" v-on:submit="sendMessage">
            <span class="messageUsername">{{username}}</span>
            <input type="text" v-model="msg">
            <button v-on:click="sendMessage" v-bind:disabled="!msg" class="submitButton">Send</button>
        </form>
    </div>
</template>

<script>
export default {
    name: 'chatroom',
    props: ['messages', 'username'],
    data: function() {
        return {
            msg: ""
        }
    },
    methods: {
        sendMessage: function() {
            if(!this.msg) {
                alert("Please enter a message");
                return;
            }

            this.$emit('sendMessage', this.msg);
            this.msg = "";
        }
    }
}
</script>

<style lang="scss" scoped>
.chat-window {
    flex: 1;
    display: flex;
    flex-direction: column;
    background-color: black;
    box-shadow: 1px 1px 6px 0px rgba(0, 0, 0, 0.15);

    .messages::-webkit-scrollbar {
        color: #6441A4;
        background-color: rgba(255, 255, 255, 0.05);
    }

    .messages::-webkit-scrollbar-thumb {
        background-color: #6441A4;
    }

    .messages {
        flex: 1;
        overflow: scroll;
        overflow-x: hidden;
        max-height: 400px;

        .message {
            display: flex;
            border-bottom: 1px solid #EFEFEF;
            padding: 10px;

            &:last-of-type {
                border-bottom: none;
            }

            .currentUser {
                color: red;
                margin-right: 15px;
                font-weight: bold;
                font-size: 16px;
            }

            .username {
                color: #6441A4;
                margin-right: 15px;
                font-weight: bold;
                font-size: 16px;
            }

            .message-text {
                flex: 1;
            }

            .deleteIcon {
                margin: 0 1em;
            }
        }
    }
    
    .input-container {
        display: flex;

        .messageUsername {
            color: #6441A4;
            padding: 8px;
        }
    
        input {
            flex: 1;
            height: 35px;
            font-size: 18px;
            box-sizing: border-box;
        }

        button {
            width: 75px;
            height: 35px;
            box-sizing: border-box;
        }

        .submitButton {
            color: #6441A4;
            background-color: black;
            border: 1px solid #6441A4;
            border-radius: 5px;
            
            &:hover {
                background-color: white;
                color: black;
            }
        }
    }

    .fas, fa-trash-alt {
        text-decoration: none;
        color: grey;
    }
}
</style>