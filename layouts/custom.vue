<template>
  <div class="d-flex">
    <section>
      <Sidebar />
    </section>
    <main class="w-100 h100vh"><nuxt /></main>
  </div>
</template>

<script>
import Sidebar from "../components/layouts-components/Sidebar.vue";
export default {
  components: { Sidebar },
};
</script>

<style lang="scss" scoped>
.h100vh {
  height: 100vh;
}
</style>

