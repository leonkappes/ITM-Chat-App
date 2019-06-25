<template>
  <div class="container chat">
    <h2 class="text-primary text-center">TeleApp</h2>
    <h5 class="text-secondary text-center">
      Erstellt von Leon Kappes, Lennart Münter und Andreas Reiß für ITM 2019
    </h5>
    <div class="card">
      <div class="card-body">
        <p v-if="messages.length == 0" class="text-secondary nomessages">
          [Keine Nachrichten bisher!]
        </p>
        <div v-chat-scroll="{ always: false, smooth: true }" class="messages">
          <div v-for="message in messages" :key="message.id">
            <span class="text-info">[{{ message.name }}]: </span>
            <span class="text-white">{{ message.message }}</span>
            <span class="text-secondary time">{{ message.timestamp }}</span>
          </div>
        </div>
      </div>
      <div class="card-action">
        <CreateMessage :name="$route.params.name" />
      </div>
    </div>
  </div>
</template>

<script>
import CreateMessage from '@/components/CreateMessage'
import fb from '@/firebase/init'
import moment from 'moment'

export default {
  name: 'Chat',
  components: {
    CreateMessage
  },
  props: ['name'], // eslint-disable-line
  data() {
    return {
      messages: []
    }
  },
  created() {
    if (this.$route.params.name === undefined) {
      this.$router.push({ name: 'index' })
    }
    const ref = fb.collection('messages').orderBy('timestamp')
    ref.onSnapshot(snapshot => {
      snapshot.docChanges().forEach(change => {
        if (change.type == 'added') { // eslint-disable-line
          const doc = change.doc
          this.messages.push({
            id: doc.id,
            name: doc.data().name,
            message: doc.data().message,
            timestamp: moment(doc.data().timestamp).format('LTS')
          })
        }
      })
    })
  }
}
</script>
<style scoped>
.chat{
border:1px solid white;
border-radius: .25rem;
}
.form-control{
color:#000;
}
html{
overflow:hidden;
}
.chat h5{
margin-bottom:3%;
}
.chat h2{
margin-top:3%;
}
h2{
font-size: 3rem;
}
h5{
font-size: 1rem;
}
.card {
  background-color: rgb(68,68,70);
  border:0;
}
.card-body{
border:1px solid rgb(64,64,62);
border-radius: .25rem;
padding: 0;
}
.container {
    background-color:rgb(68,68,70);
    width: 95%;
    padding-right: 15px;
    padding-left: 15px;
    margin-right: auto;
    margin-left: auto;
    margin-top: 2.3%;
}
.messages{
height:64.6vh;
overflow:auto;
max-height:auto;
}
.text-primary{
color:#009eed!important;
}
.text-info {
    color: #1fb4ff!important;
}
.chat span {
  font-size: 1.2em;
}
.chat .time {
  display: block;
  font-size: 0.7em;
}
.messages {
  max-height: 300px;
  overflow: auto;
}
</style>
