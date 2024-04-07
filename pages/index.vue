<template>
  <div class="">
    <div class="w-100 text-center">
      <img
        class="w-150px m-5"
        src="@/assets/img/tfgmpclogo.jpg"
        alt="logo Image"
      />
      <div class="font-weight-bold fsize-large">
        {{ appname }}
      </div>

      <div class="font-weight-bold fsize-xlarge">
        FINANCIAL MANAGEMENT SYSTEM
      </div>
      <span> {{ version }} </span>
    </div>
    <!-- LOGIN FORM -->
    <div class="login-form mt-5">
      <b-form>
        <b-form-input
          id="username"
          size="lg"
          class="mb-2"
          placeholder="Username"
          v-model="username"
          type="text"
        ></b-form-input>
        <b-form-input
          id="password"
          size="lg mb-2"
          placeholder="Password"
          type="password"
          v-model="password"
        ></b-form-input>
        <b-button
          size="lg"
          class="w-100"
          variant="primary"
          @click="navigateToOtherRoute()"
        >
          Sign-In
          <font-awesome-icon :icon="['fas', 'arrow-right-to-bracket']" />
        </b-button>
      </b-form>
    </div>
  </div>
</template>

<script>
export default {
  name: "IndexPage",
  data() {
    return { username: "", password: "", appname: "", version: "" };
  },
  methods: {
    navigateToOtherRoute() {
      this.$router.push("/admin");
      localStorage.selectedMenu = "Admin";
    },
  },

  mounted() {
    this.appname = process.env.APPNAME;
    this.version = process.env.VERSION;
  },
};
</script>

<style scoped lang="scss">
.login-form {
  margin-left: 40%;
  margin-right: 40%;
}
</style>
