<template>
  <div class="container chat">
    <h2 class="text-primary text-center">Real-Time Chat</h2>
    <h5 class="text-secondary text-center">
      Erstellt von Leon Kappes, Lennart Münter und Andreas Reiß für ITM 2019
    </h5>
    <div class="card bg">
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
.chat h2 {
  font-size: 2.6em;
  margin-bottom: 0px;
  margin-top: 50px;
}
.chat h5 {
  margin-top: 0px;
  margin-bottom: 40px;
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
