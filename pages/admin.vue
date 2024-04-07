<template>
  <div class="p-3 h-100">
    <b-card no-body class="h-100 page-card">
      <b-tabs
        card
        small
        active-nav-item-class="font-weight-bold text-uppercase text-success"
      >
        <b-tab title="Vendor" active><Vendor /></b-tab>
        <b-tab title="Customer"> </b-tab>
      </b-tabs>
    </b-card>
  </div>
</template>
<script>
import Vendor from "../components/admin-tabs/Vendor.vue";
export default {
  components: { Vendor },
  layout: "custom",
};
</script>
<style lang="scss" scoped>
a:hover {
  color: #00a651;
  text-decoration: underline;
  background-color: rgb(250, 250, 250);
}
</style>