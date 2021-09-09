<template>
  <div class="app-container">
    <div class="top"></div>
    <div class="top-shadow"></div>
    <div class="leds">
      <div></div>
      <div></div>
      <div></div>
    </div>
    <div class="button"></div>
    <div class="screen">
      <div v-if="chats.length > 0">
        <chat-card v-for="chat in chats"
          :username="chat.user"
          :message="chat.chat"
          :isSub="chat.sub"
          :key="chat"
        />
      </div>
      <p v-else>This user is not online</p>
    </div>
  </div>
</template>

<script>

import tmi from 'tmi.js';
import ChatCard from './components/ChatCard.vue';

export default {
  name: 'App',
  components: {
    ChatCard,
  },
  data() {
    return {
      chats: [],
    };
  },
  created() {
    let channelArray = ['f8rdy'];
    if (window.location.search) {
      channelArray = [window.location.search.split('=')[1]];
    }
    const client = new tmi.Client({
      channels: channelArray,
    });
    client.connect();

    client.on('message', (channel, tags, message) => {
      this.chats.unshift({ user: tags['display-name'], chat: message, sub: tags.subscriber });
      if (this.chats.length >= 50) {
        const elementsToRemove = Math.floor(this.chats.length / 2);
        this.chats.splice(this.chats.length - elementsToRemove, elementsToRemove);
      }
    });
  },
};
</script>

<style>
*{
  box-sizing: border-box;
}
#app {
  font-family: 'Nunito', sans-serif;
  text-align: center;
  color: #2c3e50;
  margin: auto 0;
  width: 100%;
  height: 90vh;
  overflow-y: hidden;
  display: flex;
  justify-content: center;
}
.app-container{
  position: relative;
  height: 100%;
  overflow: hidden;
  background: rgb(204, 44, 44);
  width: 25rem;
  border-radius: 20px;
  box-shadow: 0 0 0 10px rgb(163, 33, 33) inset;
}
.top{
  width: 100%;
  height: 30%;
  z-index: 5;
  position: absolute;
  background: rgb(204, 44, 44);
  clip-path: polygon(0 0, 100% 0, 100% 55%, 60% 55%, 40% 98%, 0 98%);
}
.top-shadow {
  width: 100%;
  height: 30%;
  z-index: 4;
  position: absolute;
  background: rgb(163, 33, 33);
  clip-path: polygon(0 0, 100% 0, 100% 58%, 60% 58%, 42% 100%, 0 100%);
}
.button{
  position: absolute;
  width: 20%;
  height: 12%;
  top: 10%;
  left: 10%;
  transform: translate(-10%, -10%);
  z-index: 8;
  background: rgba(12, 102, 175, 0.973);
  border-radius: 50%;
  box-shadow: 0 0 0 5px gray;
}
.leds{
  position: absolute;
  top: 5px;
  left: 50%;
  transform: translateX(-50%);
  z-index: 9;
  display: flex;
}
.leds div{
  width: 20px;
  height: 20px;
  margin: 5px;
  border-radius: 50%;
}
.leds :nth-child(1){
  background: rgb(197, 48, 197);
}
.leds :nth-child(2){
  background: rgb(224, 224, 57);
}
.leds :nth-child(3){
  background: rgb(48, 197, 48);
}

.screen{
  width: 80%;
  margin: auto;
  height: 60%;
  position: absolute;
  top: 62%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 10;
  background: black;
  padding: 20px;
  clip-path: polygon(0 0, 100% 0, 100% 55%, 100% 100%, 29% 100%, 0 73%);
  box-shadow: 0 0 0 15px gray inset;
  padding: 20px 15px;
}
</style>
