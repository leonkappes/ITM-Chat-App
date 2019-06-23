<template>
  <div class="container" style="margin-bottom: 30px">
    <b-form class="text-center" @submit.prevent="createMessage">
      <b-input
        id="user"
        v-model="newMessage"
        class="text-white bg"
        placeholder="Deine Nachricht..."
        :state="validation"
        autocomplete="off"
      ></b-input>
      <b-form-invalid-feedback :state="validation">
        Deine Nachricht muss mindestens 1 Zeichen Lang sein!
      </b-form-invalid-feedback>
      <b-form-valid-feedback :state="validation">
        Sieht gut aus!
      </b-form-valid-feedback>
      <b-button type="submit" variant="primary">Submit</b-button>
    </b-form>
  </div>
</template>

<script>
import fb from '@/firebase/init'

const consola = require('consola')
export default {
  name: 'CreateMessage',
  props: ['name'], // eslint-disable-line
  data() {
    return {
      newMessage: ''
    }
  },
  computed: {
    validation() {
      return this.newMessage.length >= 1
    }
  },
  methods: {
    createMessage() {
      if (this.newMessage.length >= 1) {
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
input[type='text']:focus {
  background-color: #272727;
}
</style>
