<template>
  <div class="container bg">
    <div class="card login bg text-white">
      <div class="card-body">
        <h2 class="card-title text-center">Login</h2>
        <b-form class="text-center" @submit.prevent="login">
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
.card{
border:1px solid white;
background-color:rgb(68,68,70);
}
.form-control.is-invalid{
background-color:white;
border-color:#f00;	
color:#000!important;
}
.form-control.is-valid{
color:#000!important;
border-color:rgb(52,199,89);
}
h2 {
font-size:3rem;
}
.invalid-feedback {
margin-top: .4rem;
color:#f00;
}
.btn-primary{
background-color: rgb(10,132,255);
border-color: rgb(10,132,255);
}
.btn{
margin-top: .4rem;
margin-bottom: .75rem;
}
.btn-primary:hover {
background-color: rgb(45,90,230);
border-color: rgb(45,90,230);
}
.form-control:focus {
    color: #495057;
    background-color: #fff;
    border-color: rgb(52,199,89);
    outline: 0;
    box-shadow: 0 0 0 0.2rem rgba(40,167,69,.25);
}
.valid-feedback{
margin-top: .4rem;
color:rgb(52,199,89);
}
</style>
