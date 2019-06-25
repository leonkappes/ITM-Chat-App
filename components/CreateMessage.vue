<template>
  <div class="container" style="margin-bottom: 30px">
    <b-form class="text-center" @submit.prevent="createMessage">
      <b-input
        id="user"
        v-model="newMessage"
        placeholder="Deine Nachricht..."
        autocomplete="off"
      ></b-input>
      <b-button type="submit" variant="primary">Submit</b-button>
    </b-form>
  </div>
</template>

<script>
import fb from '@/firebase/init'
import consola from 'consola'

export default {
  name: 'CreateMessage',
  props: ['name'], // eslint-disable-line
  data() {
    return {
      newMessage: ''
    }
  },
  methods: {
    createMessage() {
      if (this.newMessage.length > 0) {
        fb.collection('messages')
          .add({
            message: this.newMessage,
            name: this.name,
            timestamp: Date.now()
          })
          .catch(err => {
            consola.error(err)
          })
        this.newMessage = ''
      }
    }
  }
}
</script>
<style scoped>
</style>
