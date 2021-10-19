<template>
    <div class="container">
      <div class="row justify-content-center">
        <div class="col-12 col-md-6 col-xl-6 my-5">
            <SignupForm />
        </div>
      </div>
    </div>
</template>
<script>
import SignupForm from '../components/SignupForm.vue'

export default {
  name: 'Home',
  components: {
    SignupForm
  }
}
</script>