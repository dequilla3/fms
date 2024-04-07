<template>
  <!-- TOOLTIP -->
  <div class="sidebar-tooltip pl-1">
    <div class="pl-4">
      <div class="bg-white p-4 border">
        <b-button pill class="w-100 mb-2" variant="outline-secondary">
          <font-awesome-icon :icon="['fas', 'user']" /> View Profile
        </b-button>
        <b-button
          @click="onLogout()"
          pill
          class="w-100"
          variant="outline-secondary"
        >
          <font-awesome-icon :icon="['fas', 'right-from-bracket']" />
          Logout
        </b-button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  methods: {
    onLogout() {
      this.$router.push("/");
    },
  },
};
</script>

<style scoped>
.sidebar-tooltip {
  z-index: 1;
  left: 140px;
  position: fixed;
  width: 20vw;
  background: skyblue;

  clip-path: polygon(
    6% 0,
    100% 0,
    100% 43%,
    100% 100%,
    68% 100%,
    32% 100%,
    6% 99%,
    6% 18%,
    0 11%
  );
}
</style>