<template>
  <div class="card">
    <h3 class="card-header text-center">Register</h3>
    <div class="card-body">
      <form>
        <div class="form-row">
          <div class="form-group col-md-6">
            <label>First Name</label>
            <input type="text" class="form-control"
            v-model.trim="$v.firstname.$model"
            :class="{'is-invalid': $v.firstname.$error, 'is-valid': !$v.firstname.$invalid }">
            <div class="valid-feedback">Your first name is valid</div>
            <div class="invalid-feedback">
              <span v-if="!$v.firstname.required">First name is required</span>
              <span v-if="!$v.firstname.minLength">
                First name must be at least {{ $v.firstname.$params.minLength.min }} letters
              </span>
              <span v-if="!$v.firstname.maxLength">
                First name must have at most {{ $v.firstname.$params.maxLength.max }} letters
              </span>
            </div>
          </div>
          <div class="form-group col-md-6">
            <label>Last Name</label>
            <input type="text" class="form-control"
            v-model.trim="$v.lastname.$model"
            :class="{'is-invalid': $v.lastname.$error, 'is-valid': !$v.lastname.$invalid }">
            <div class="valid-feedback">Your Last name is valid</div>
            <div class="invalid-feedback">
              <span v-if="!$v.lastname.required">Last name is required</span>
              <span v-if="!$v.lastname.minLength">
                Last name must be at least {{ $v.lastname.$params.minLength.min }} letters
              </span>
              <span v-if="!$v.lastname.maxLength">
                Last name must have at most {{ $v.lastname.$params.maxLength.max }} letters
              </span>
            </div>
          </div>
        </div>
        <div class="form-group">
          <label>Username</label>
          <input type="text" class="form-control"
          v-model.trim="$v.username.$model"
          :class="{'is-invalid': $v.username.$error, 'is-valid': !$v.username.$invalid }">
          <div class="valid-feedback">Your username is valid</div>
          <div class="invalid-feedback">
          <span v-if="!$v.username.required">Username is required</span>
          <span v-if="!$v.username.minLength">
            Your username must be at least {{$v.username.$params.minLength.min}} characters
          </span>
          <span v-if="!$v.username.isUnique">This username is already taken</span>
          </div>
        </div>
        <div class="form-group">
          <label>Email</label>
          <input type="email" class="form-control"
          v-model.trim="$v.email.$model"
          :class="{'is-invalid': $v.email.$error, 'is-valid': !$v.email.$invalid }">
          <div class="valid-feedback">Your email is valid</div>
          <div class="invalid-feedback">
          <span v-if="!$v.email.required">Email is required</span>
          <span v-if="!$v.email.isUnique">Your email is invalid or already taken</span>
          </div>
        </div>
        <div class="form-group">
          <label>Password</label>
          <input type="password" id="password" class="form-control"
          v-model.trim="$v.password.$model"
          :class="{'is-invalid': $v.password.$error, 'is-valid': !$v.password.$invalid }">
          <div class="valid-feedback">Your password is valid</div>
          <div class="invalid-feedback">
          <span v-if="!$v.password.required">Password is required</span>
          <span v-if="!$v.email.minLength">
            {{ $v.password.$params.minLength.min }} characters minimum
            </span>
          </div>
        </div>

        <div class="form-group">
          <div class="form-check">
            <input class="form-check-input" type="checkbox"
            id="showPassword" @click="toggleShowPassword" v-model="showPassword">
            <label class="form-check-label" for="showPassword">
              Show Password
            </label>
          </div>
        </div>

        <div class="form-group">
          <label>Repeat Password</label>
          <input type="password" class="form-control"
          v-model.trim="$v.repeatPassword.$model"
          :class="{'is-invalid': $v.repeatPassword.$error,
          'is-valid': (password != '') ? !$v.repeatPassword.$invalid : '' }">

          <div class="valid-feedback">Your passwords are identical</div>
          <div class="invalid-feedback">
            <span v-if="!$v.repeatPassword.sameAsPassword">
              Passwords must be identical
            </span>
          </div>
        </div>
        <div class="form-group">
          <label>Age</label>
          <input type="number" class="form-control"
          v-model.number.lazy="$v.age.$model"
          :class="{'is-invalid': $v.age.$error, 'is-valid': !$v.age.$invalid }">
          <div class="valid-feedback">Your Age is valid</div>
          <div class="invalid-feedback">
          <span v-if="!$v.age.required">Age is required</span>
          <span v-if="!$v.age.between">
            Must be between {{ $v.age.$params.between.min }} and {{ $v.age.$params.between.max }}
          </span>
          </div>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import { required, minLength, maxLength, between, email, sameAs } from 'vuelidate/lib/validators'

export default {
  name: 'FormValidation',
  data() {
    return {
      firstname: '',
      lastname: '',
      username: '',
      email: '',
      age: 0,
      password: '',
      repeatPassword: '',
      showPassword: false
    }
  },
  validations: {
    firstname: {
      required,
      minLength: minLength(3),
      maxLength: maxLength(15)
    },
    lastname: {
      required,
      minLength: minLength(3),
      maxLength: maxLength(20)
    },
    username: {
      required,
      minLength: minLength(3),
      isUnique (value) {
        if(value === '') return true

        return new Promise((resolve) => {
          setTimeout(() => {
            resolve(typeof value === 'string' && value.length % 2 !== 0)
          }, 350 + Math.random() * 300)
        })
      }
    },
    email: {
      required,
      email,
      isUnique (value) {
        if(value === '') return true

        // eslint-disable-next-line
        var email_regex = /^(([^<>()\[\]\\.,;:\s@"]+(\.[^<>()\[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/;
        return new Promise((resolve) => {
          setTimeout(() => {
            resolve(email_regex.test(value))
          }, 350 + Math.random() * 300)
        })
      }
    },
    age: {
      between: between(16, 80)
    },
    password: {
      required,
      minLength: minLength(8)
    },
    repeatPassword: {
      sameAsPassword: sameAs('password')
    }
  },
  methods: {
    toggleShowPassword() {
      var show = document.getElementById('password');
      if(this.showPassword == false) {
        this.showPassword = true;
        show.type = 'text'
      } else {
        this.showPassword = false;
        show.type = 'password'
      }
    }
  }
}
</script>

<style scoped>

</style>
