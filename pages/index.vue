<template>
  <div class="container bg">
    <div class="card login bg text-white">
      <div class="card-body">
        <h2 class="card-title text-center">Login</h2>
        <b-form class="text-center" @submit.prevent="login">
          <label for="user">Username</label>
          <b-input
            id="user"
            v-model="name"
            class="text-white bg"
            placeholder="Dein Name"
            :state="validation"
          ></b-input>
          <b-form-invalid-feedback :state="validation">
            Dein Name muss mindestens 3 Zeichen Lang sein!
          </b-form-invalid-feedback>
          <b-form-valid-feedback :state="validation">
            Sieht gut aus!
          </b-form-valid-feedback>
          <b-button type="submit" variant="primary">Submit</b-button>
        </b-form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Login',
  data() {
    return {
      name: '',
      err: null
    }
  },
  computed: {
    validation() {
      return this.name.length >= 3
    }
  },
  methods: {
    login() {
      // TODO: Add check if user exist(maybe popper.js?!?)
      if (this.name) {
        this.$router.push({ name: 'Chat', params: { name: this.name } })
      }
    }
  }
}
</script>
<style scoped>
.login {
  max-width: 450px;
  margin-top: 50px;
  display: block;
  margin-left: auto;
  margin-right: auto;
}
input[type='text']:focus {
  background-color: #272727;
}
</style>
