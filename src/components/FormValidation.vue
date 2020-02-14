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
import { required, minLength, maxLength, between } from 'vuelidate/lib/validators'

export default {
  name: 'FormValidation',
  data() {
    return {
      firstname: '',
      lastname: '',
      age: 0
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
    age: {
      between: between(16, 80)
    }
  }
}
</script>

<style scoped>

</style>
