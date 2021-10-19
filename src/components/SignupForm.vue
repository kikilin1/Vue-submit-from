<template>
  <form id="signup-form" v-on:submit.prevent="submit">
    <div class="row">
      <div class="col-12 form-group">
        <label class="col-form-label col-form-label-lg"
          >First Name <span class="text-danger">*</span></label
        >
        <input
          type="text"
          v-model.trim="$v.firstname.$model"
          :class="{ 'is-invalid': validationStatus($v.firstname) }"
          class="form-control form-control-lg"
        />
        <small v-if="!$v.firstname.required" class="invalid-feedback">
          The first name field is required.
        </small>
      </div>
      <div class="col-12 form-group">
        <label class="col-form-label col-form-label-lg"
          >Last Name <span class="text-danger">*</span></label
        >
        <input
          type="text"
          v-model.trim="$v.lastname.$model"
          :class="{ 'is-invalid': validationStatus($v.lastname) }"
          class="form-control form-control-lg"
        />
        <small v-if="!$v.lastname.required" class="invalid-feedback">
          The last name field is required.
        </small>
      </div>
      <div class="col-12 form-group">
        <label class="col-form-label col-form-label-lg">
          Prefer Language <span class="text-danger">*</span></label
        >
        <select
          v-model.trim="$v.language.$model"
          :class="{ 'is-invalid': validationStatus($v.language) }"
          class="form-control form-control-lg"
        >
          <option value="">Select language</option>
          <option
            :value="lang.code"
            :key="lang.id"
            v-for="lang in languageList"
          >
            {{ lang.name }}
          </option>
        </select>
        <small v-if="!$v.language.required" class="invalid-feedback">
          The language field is required.
        </small>
      </div>
      <div class="col-12 form-group">
        <label class="col-form-label col-form-label-lg"
          >Email <span class="text-danger">*</span></label
        >
        <input
          type="email"
          v-model.trim="$v.email.$model"
          :class="{ 'is-invalid': validationStatus($v.email) }"
          class="form-control form-control-lg"
        />
        <small v-if="!$v.email.required" class="invalid-feedback">
          The email field is required.
        </small>
        <small v-if="!$v.email.email" class="invalid-feedback">
          The email is not valid.
        </small>
      </div>
      <div class="col-12 form-group">
        <label class="col-form-label col-form-label-lg"
          >Password <span class="text-danger">*</span></label
        >
        <input
          type="password"
          v-model.trim="$v.password.$model"
          :class="{ 'is-invalid': validationStatus($v.password) }"
          class="form-control form-control-lg"
        />
        <small v-if="!$v.password.required" class="invalid-feedback">
          The password field is required.
        </small>
        <small v-if="!$v.password.minLength" class="invalid-feedback">
          You must have at least
          {{ $v.password.$params.minLength.min }} letters.
        </small>
        <small v-if="!$v.password.maxLength" class="invalid-feedback">
          You must not have greater then
          {{ $v.password.$params.maxLength.min }} letters.
        </small>
      </div>
      <div class="col-12 form-group text-center">
        <button class="btn btn-primary">Submit</button>
      </div>
    </div>
  </form>
</template>
<script>
import {
  required,
  email,
  minLength,
  maxLength,
} from "vuelidate/lib/validators";
export default {
  name: "SignupForm",
  data: function () {
    return {
      firstname: "",
      lastname: "",
      language: "",
      email: "",
      password: "",
      languageList: [],
    };
  },
  validations: {
    firstname: { required },
    lastname: { required },
    language: { required },
    email: { required, email },
    password: { required, minLength: minLength(6), maxLength: maxLength(18) },
  },

  mounted() {
    //get language list api
    var v = this;
    v.$http
      .get(`https://api.coindirect.com/api/language`)
      .then(function (res) {
        v.languageList = res.data;
      })
      .catch(function (err) {
        console.log(err);
      });
  },

  methods: {
    resetData() {
      this.firstname = "";
      this.lastname = "";
      this.language = "";
      this.email = "";
      this.password = "";
    },

    validationStatus(validation) {
      return typeof validation != "undefined" ? validation.$error : false;
    },

    submit() {
      this.$v.$touch();
      if (this.$v.$error) return;
      //success clean the form, go to next page.
      this.$v.$reset();
      this.resetData();
      this.$router.push({
        name: "Welcome",
      });
    },
  },
};
</script>
<style>
</style>