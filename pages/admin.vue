<template>
  <div class="p-3 h-100">
    <b-card no-body class="h-100 page-card">
      <b-tabs
        card
        small
        active-nav-item-class="font-weight-bold text-uppercase text-success"
      >
        <b-tab title="Vendor" active><Vendor /></b-tab>
        <b-tab title="CUSTOMER"><Customer /></b-tab>
        <b-tab title="FIELD"><Fields /></b-tab>
      </b-tabs>
    </b-card>
  </div>
</template>
<script>
import Vendor from "../components/admin-tabs/Vendor.vue";
import Customer from "../components/admin-tabs/Customer.vue";
import Fields from "../components/admin-tabs/Fields.vue";

export default {
  components: { Vendor, Customer, Fields },
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