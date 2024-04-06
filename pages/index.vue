<template>
  <div class="">
    <div class="w-100 text-center">
      <img
        class="w-150px m-5"
        src="@/assets/img/tfgmpclogo.jpg"
        alt="logo Image"
      />
      <div class="font-weight-bold fsize-large">TFGMPC</div>
      <div class="font-weight-bold fsize-xlarge">
        FINANCIAL MANAGEMENT SYSTEM
      </div>
    </div>
    <!-- LOGIN FORM -->
    <div class="login-form mt-5">
      <b-form>
        <b-form-input
          id="input-lg"
          size="lg"
          class="mb-2"
          placeholder="Username"
        ></b-form-input>
        <b-form-input
          id="input-lg"
          size="lg mb-2"
          placeholder="Password"
        ></b-form-input>
        <b-button size="lg" class="w-100" variant="primary">
          Login
          <font-awesome-icon :icon="['fas', 'arrow-right-to-bracket']" />
        </b-button>
      </b-form>
    </div>
  </div>
</template>

<script>
export default {
  name: "IndexPage",
};
</script>

<style scoped lang="scss">
.login-form {
  margin-left: 40%;
  margin-right: 40%;
}
</style>
