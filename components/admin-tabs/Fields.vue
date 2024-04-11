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

      <b-button @click="addField()" variant="primary">
        <font-awesome-icon :icon="['fas', 'plus']" />
        Add field
      </b-button>
    </div>

    <b-table
      show-empty
      class="table-style"
      scrollable
      sticky-header
      no-border-collapse
      :fields="fieldFields"
      :items="fieldList"
      hover
    >
      <template v-slot:cell(fieldId)="row">
        {{ "FID" + String(row.item.fieldId).padStart(5, "0") }}
      </template>

      <template v-slot:cell(actions)="row">
        <b-button
          size="sm"
          class="pl-3 pr-3"
          @click="showDetailsModal(row.item)"
        >
          <font-awesome-icon :icon="['fas', 'eye']" />
        </b-button>
      </template>
    </b-table>

    <b-modal
      id="fieldDetailsModal"
      size="smed"
      header-bg-variant="primary"
      header-text-variant="light"
      body-bg-variant="gray"
      scrollable
      hide-footer
    >
      <template v-slot:modal-title>
        <h6>Field Details</h6>
      </template>

      <div class="ml-5 mr-5">
        <div
          v-if="!isAdd"
          class="position-fixed bg-light rounded shadow-sm m-3"
        >
          <b-form-checkbox
            v-model="isEditMode"
            class="m-1"
            switch
            @click="onClickEditMode()"
          >
            Edit Mode
          </b-form-checkbox>
        </div>

        <div v-if="!isAdd"><br /><br /></div>

        <hr />

        <!-- 1 -->
        <div class="justify-content-between">
          <div class="m-3">
            <small class="text-left">Description</small>
            <b-form-input
              :disabled="!isEditMode"
              type="text"
              :maxlength="10"
              class="form-text"
              v-model="fieldData.desc"
            />
          </div>
          <div class="m-3">
            <small class="text-left">Remarks</small>
            <b-form-input
              :disabled="!isEditMode"
              type="text"
              :maxlength="10"
              class="form-text"
              placeholder="(Optional)"
              v-model="fieldData.remarks"
            />
          </div>
        </div>

        <hr />

        <!-- footer -->
        <div class="d-flex justify-content-between" v-if="isEditMode">
          <div class="w-25 m-3">
            <b-button variant="primary" class="w-100">Save</b-button>
          </div>
          <div class="w-25 m-3">
            <b-button @click="$bvModal.hide('fieldDetailsModal')" class="w-100">
              Close
            </b-button>
          </div>
          <div class="w-25 m-3"></div>
        </div>
      </div>
    </b-modal>
  </div>
</template>
<script>
import { FIELD_DATA } from "../../test/data/constants.js";
import { FIELD, fieldFields } from "../../model/field";
export default {
  data() {
    return {
      fieldFields,
      fieldList: FIELD_DATA,
      fieldData: FIELD,
      isEditMode: false,
      isAdd: false,
    };
  },

  methods: {
    onClickEditMode() {
      this.isEditMode = !this.isEditMode;
    },

    showDetailsModal(item) {
      this.isEditMode = false;
      this.isAdd = false;
      this.fieldData = FIELD;
      this.$bvModal.show("fieldDetailsModal");
      this.setFieldDetails(item);
    },

    addField() {
      this.isEditMode = true;
      this.isAdd = true;
      this.$bvModal.show("fieldDetailsModal");
      this.fieldData = {
        fieldId: "",
        desc: "",
        remarks: "",
        dateCreated: "",
        viewDetails: "",
      };
    },

    setFieldDetails(item) {
      this.fieldData.desc = item.desc;
      this.fieldData.remarks = item.remarks;
      this.fieldData.dateCreated = item.dateCreated;
    },
  },
};
</script>
<style lang="sass" scoped>
</style>