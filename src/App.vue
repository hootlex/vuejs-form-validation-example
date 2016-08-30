<template>
  <div id="app">
    <form @submit.prevent="validateBeforeSubmit" v-if="!formSubmitted">

        <h1>Form Validation with Vue 2</h1>
        <div class="form-group" :class="{'has-error': errors.has('name') }">
            <label class="control-label" for="name">Name</label>
            <input v-model="name" v-validate.initial="name" data-rules="required|alpha|min:3" class="form-control" type="text" placeholder="Full Name">
            <p class="text-danger" v-if="errors.has('name')">{{ errors.first('name') }}</p>
        </div>
        <div class="form-group" :class="{'has-error': errors.has('email') }" >
            <label class="control-label" for="email">Email</label>
            <input v-model="email" v-validate.initial="email" data-rules="required|email" class="form-control" type="email" placeholder="Email">
            <p class="text-danger" v-if="errors.has('email')">{{ errors.first('email') }}</p>
        </div>
        <div class="form-group" :class="{'has-error': errors.has('url') }">
            <label class="control-label" for="url">Website</label>
            <input v-model="url" v-validate.initial="url" data-rules="required|url" class="form-control" type="text" placeholder="Website">
            <p class="text-danger" v-if="errors.has('url')">{{ errors.first('url') }}</p>
        </div>
        <div class="form-group" :class="{'has-error': errors.has('secret') }">
            <label class="control-label" for="secret">Secret</label>
            <input v-model="secret" v-validate.initial="secret" data-rules="required|passphrase" class="form-control" type="text" placeholder="Code Phrase">
            <p class="text-danger" v-if="errors.has('secret')">{{ errors.first('secret') }}</p>
        </div>

        <button class="btn btn-primary btn-block" type="submit">Submit</button>
    </form>
    <div v-else>
      <h1 class="submitted">Form submitted successfully!</h1>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import VeeValidate from 'vee-validate';

Vue.use(VeeValidate);

VeeValidate.Validator.extend('passphrase', {
    getMessage: field => 'Sorry dude, wrong pass phrase.',
    validate: value => value.toUpperCase() == 'Demogorgon'.toUpperCase()
});

export default {
  data () {
    return {
      email: '',
      name: '',
      url: '',
      secret: '',
      formSubmitted: false
    }
  },
  methods: {
    validateBeforeSubmit(e) {
        this.$validator.validateAll();

        if (!this.errors.any()) {
            this.submitForm()
        }
      },
    submitForm(){
      this.formSubmitted = true
    }
  }
}
</script>

<style>
body {
  font-family: Helvetica, sans-serif;
}
.container {
  width: 500px;
}
h1 {
  text-align: center
}
img {
  text-align: center
}
.submitted {
  color: #4fc08d;
}
</style>
