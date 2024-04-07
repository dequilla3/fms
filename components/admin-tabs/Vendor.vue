<template>
  <div>
    <div class="d-flex justify-content-between">
      <div class="d-flex w-25">
        <b-form-input
          id="inline-form-input-search"
          placeholder="Search..."
          class="mr-1"
        />
        <b-button class="w-25" variant="primary">
          <font-awesome-icon :icon="['fas', 'magnifying-glass']" />
        </b-button>
      </div>

      <b-button class="" variant="primary">
        <font-awesome-icon :icon="['fas', 'plus']" />
        Add vendor
      </b-button>
    </div>
  </div>
</template>
<script>
export default {};
</script>
<style lang="sass" scoped>
</style>