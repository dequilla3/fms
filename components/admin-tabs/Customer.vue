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

      <b-button @click="addCustomer()" variant="primary">
        <font-awesome-icon :icon="['fas', 'plus']" />
        Add customer
      </b-button>
    </div>

    <b-table
      show-empty
      class="table-style"
      scrollable
      sticky-header
      no-border-collapse
      :fields="customerFields"
      :items="customerList"
      hover
    >
      <template v-slot:cell(customerId)="row">
        {{ "CUST" + String(row.item.customerId).padStart(5, "0") }}
      </template>

      <template v-slot:cell(fullName)="row">
        {{ row.item.firstName + " " + row.item.lastName }}
      </template>

      <template v-slot:cell(actions)="row">
        <b-button
          size="sm"
          class="pl-3 pr-3"
          @click="showDetailsModal(row.item)"
        >
          VIEW DETAILS
        </b-button>
        <b-button
          size="sm"
          class="pl-3 pr-3"
          @click="onAssignBtnClick(row.item)"
        >
          ASSIGN FIELD
        </b-button>
      </template>
    </b-table>

    <b-modal
      id="customerDetailsModal"
      size="huge"
      header-bg-variant="primary"
      header-text-variant="light"
      body-bg-variant="gray"
      scrollable
      hide-footer
    >
      <template v-slot:modal-title>
        <h6>Customer Details</h6>
      </template>

      <div class="ml-5 mr-5">
        <div
          v-if="!isAdd"
          class="position-fixed bg-light rounded shadow-sm m-3"
        >
          <b-form-checkbox
            v-model="isEditMode"
            class="m-3"
            size="lg"
            switch
            @click="onClickEditMode()"
          >
            Edit Mode
          </b-form-checkbox>
        </div>

        <div v-if="!isAdd"><br /><br /><br /></div>

        <hr />

        <!-- 1 -->
        <div class="d-flex justify-content-between">
          <div class="w-25 m-3">
            <small class="text-left">First Name</small>
            <b-form-input
              :disabled="!isEditMode"
              type="text"
              :maxlength="10"
              class="form-text"
              v-model="customerData.firstName"
            />
          </div>
          <div class="w-25 m-3">
            <small class="text-left">Middle Name</small>
            <b-form-input
              :disabled="!isEditMode"
              type="text"
              :maxlength="10"
              class="form-text"
              placeholder="(Optional)"
              v-model="customerData.middleName"
            />
          </div>
          <div class="w-25 m-3">
            <small class="text-left">Last Name</small>
            <b-form-input
              :disabled="!isEditMode"
              type="text"
              :maxlength="10"
              class="form-text"
              v-model="customerData.lastName"
            />
          </div>
        </div>

        <!-- 2 -->
        <div class="d-flex justify-content-between">
          <div class="w-25 m-3">
            <small class="text-left">Contact Person</small>
            <b-form-input
              :disabled="!isEditMode"
              type="text"
              :maxlength="10"
              class="form-text"
              v-model="customerData.contactPerson"
            />
          </div>
          <div class="w-25 m-3">
            <small class="text-left">Contact Person Phone</small>
            <b-form-input
              :disabled="!isEditMode"
              type="text"
              :maxlength="10"
              class="form-text"
              v-model="customerData.contactPersonPhone"
            />
          </div>
          <div class="w-25 m-3"></div>
        </div>

        <!-- 3 -->
        <div class="d-flex justify-content-between">
          <div class="w-25 m-3">
            <small class="text-left">Street</small>
            <b-form-input
              :disabled="!isEditMode"
              type="text"
              :maxlength="10"
              class="form-text"
              v-model="customerData.street"
            />
          </div>
          <div class="w-25 m-3">
            <small class="text-left">City</small>
            <b-form-input
              :disabled="!isEditMode"
              type="text"
              :maxlength="10"
              class="form-text"
              v-model="customerData.city"
            />
          </div>
          <div class="w-25 m-3">
            <small class="text-left">Province</small>
            <b-form-input
              :disabled="!isEditMode"
              type="text"
              :maxlength="10"
              class="form-text"
              v-model="customerData.province"
            />
          </div>
        </div>

        <!-- 4 -->
        <div class="d-flex justify-content-between">
          <div class="w-25 m-3">
            <small class="text-left">ZIP Code</small>
            <b-form-input
              :disabled="!isEditMode"
              type="text"
              :maxlength="10"
              class="form-text"
              v-model="customerData.zipCode"
            />
          </div>
          <div class="w-25 m-3">
            <small class="text-left">TIN</small>
            <b-form-input
              :disabled="!isEditMode"
              type="text"
              :maxlength="10"
              class="form-text"
              placeholder="(Optional)"
              v-model="customerData.TIN"
            />
          </div>
          <div class="w-25 m-3">
            <small class="text-left">Business Type</small>
            <b-form-select
              :disabled="!isEditMode"
              id="businessType"
              v-model="customerData.businessType"
              class="form-text"
              :options="businessTypeOptions"
            ></b-form-select>
          </div>
        </div>

        <!-- 5 -->
        <div class="d-flex justify-content-between">
          <div class="w-25 m-3">
            <small class="text-left">Payment Terms</small>
            <b-form-input
              :disabled="!isEditMode"
              type="text"
              :maxlength="10"
              class="form-text"
              v-model="customerData.paymentTerms"
            />
          </div>
          <div class="w-25 m-3">
            <small class="text-left">Payment Method</small>
            <b-form-select
              :disabled="!isEditMode"
              id="businessType"
              v-model="customerData.paymentMethod"
              class="form-text"
              :options="paymentMethodOptions"
            ></b-form-select>
          </div>
          <div class="w-25 m-3">
            <small class="text-left">Product/Services</small>
            <b-form-input
              :disabled="!isEditMode"
              type="text"
              :maxlength="10"
              class="form-text"
              v-model="customerData.productServicesProvided"
            />
          </div>
        </div>

        <!-- 6 -->
        <div class="d-flex justify-content-between">
          <div class="w-25 m-3">
            <small class="text-left">Contract Start</small>
            <b-form-input
              :disabled="!isEditMode"
              type="date"
              class="form-text"
              v-model="customerData.contractStart"
            />
          </div>
          <div class="w-25 m-3">
            <small class="text-left">Contract End</small>
            <b-form-input
              :disabled="!isEditMode"
              type="date"
              class="form-text"
              v-model="customerData.contractEnd"
            />
          </div>
          <div class="w-25 m-3"></div>
        </div>

        <!-- 7 -->
        <div class="d-flex justify-content-between">
          <div class="w-25 m-3">
            <small class="text-left">Billing address 1</small>
            <b-form-input
              :disabled="!isEditMode"
              type="text"
              :maxlength="10"
              class="form-text"
              v-model="customerData.billingAddress1"
            />
          </div>
          <div class="w-25 m-3">
            <small class="text-left">Billing address 2</small>
            <b-form-input
              :disabled="!isEditMode"
              type="text"
              :maxlength="10"
              class="form-text"
              v-model="customerData.billingAddress2"
            />
          </div>
          <div class="w-25 m-3"></div>
        </div>

        <!-- 8 -->
        <div class="d-flex justify-content-between">
          <div class="w-25 m-3">
            <small class="text-left">Rating</small>
            <b-form-input
              :disabled="!isEditMode"
              type="text"
              :maxlength="10"
              class="form-text"
              v-model="customerData.rating"
            />
          </div>
          <div class="w-25 m-3">
            <small class="text-left">Notes</small>
            <b-form-input
              :disabled="!isEditMode"
              type="text"
              :maxlength="10"
              class="form-text"
              v-model="customerData.notes"
            />
          </div>
          <div class="w-25 m-3"></div>
        </div>

        <hr />

        <!-- footer -->
        <div class="d-flex justify-content-between" v-if="isEditMode">
          <div class="w-25 m-3">
            <b-button variant="primary" class="w-100">Save</b-button>
          </div>
          <div class="w-25 m-3">
            <b-button
              @click="$bvModal.hide('customerDetailsModal')"
              class="w-100"
            >
              Close
            </b-button>
          </div>
          <div class="w-25 m-3"></div>
        </div>
      </div>
    </b-modal>

    <b-modal
      id="assignFieldModal"
      size="reg"
      header-bg-variant="primary"
      header-text-variant="light"
      body-bg-variant="gray"
      scrollable
      hide-footer
    >
      <template v-slot:modal-title>
        <h6>Assign Field</h6>
      </template>
      <b-button
        variant="primary"
        class="pl-3 pr-3"
        @click="onAddFieldModalBtnClick()"
      >
        ADD FIELD
      </b-button>
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
            variant="danger"
            size="sm"
            class="pl-3 pr-3"
            @click="onRemoveAssignedField(row.item)"
          >
            REMOVE
          </b-button>
        </template>
      </b-table>
    </b-modal>

    <b-modal
      id="addFieldModal"
      size="smed"
      header-bg-variant="primary"
      header-text-variant="light"
      body-bg-variant="gray"
      scrollable
      hide-footer
    >
      <template v-slot:modal-title>
        <h6>Add Field</h6>
      </template>
      <div class="d-flex">
        <b-form-input
          id="inline-form-input-search"
          placeholder="Search..."
          class="mr-1"
        />
        <b-button class="w-25" variant="primary">
          <font-awesome-icon :icon="['fas', 'magnifying-glass']" />
        </b-button>
      </div>
      <b-table
        show-empty
        class="table-style"
        scrollable
        sticky-header
        no-border-collapse
        :fields="addFieldFields"
        :items="addFieldList"
        hover
      >
        <template v-slot:cell(fieldId)="row">
          {{ "FID" + String(row.item.fieldId).padStart(5, "0") }}
        </template>

        <template v-slot:cell(actions)="row">
          <b-button size="sm" class="pl-3 pr-3" @click="onAddField(row.item)">
            SELECT
          </b-button>
        </template>
      </b-table>
    </b-modal>
  </div>
</template>
<script>
import {
  CUSTOMER_DATA,
  FIELD_DATA,
  PAYMENT_METHODS,
} from "../../test/data/constants.js";
import { CUSTOMER, customerFields } from "../../model/customer.js";
import { fieldFields } from "../../model/field.js";
export default {
  data() {
    return {
      customerFields,
      customerList: CUSTOMER_DATA,
      customerData: CUSTOMER,
      fieldFields,
      fieldList: FIELD_DATA,
      addFieldFields: ["fieldId", "actions"],
      addFieldList: FIELD_DATA,
      businessTypeOptions: [{ value: null, text: "Please select an option" }],
      paymentMethodOptions: PAYMENT_METHODS,
      isEditMode: false,
      isAdd: false,
    };
  },

  methods: {
    onAddField(item) {},
    onAddFieldModalBtnClick() {
      this.$bvModal.show("addFieldModal");
    },
    onAssignBtnClick() {
      this.$bvModal.show("assignFieldModal");
    },
    onRemoveAssignedField(item) {},
    onClickEditMode() {
      this.isEditMode = !this.isEditMode;
    },

    showDetailsModal(item) {
      this.isEditMode = false;
      this.isAdd = false;
      this.customerData = CUSTOMER;
      this.$bvModal.show("customerDetailsModal");
      this.setVendorDetailsToInput(item);
    },

    addCustomer() {
      this.isEditMode = true;
      this.isAdd = true;
      this.$bvModal.show("customerDetailsModal");
      this.customerData = {
        firstName: "",
        middleName: "",
        lastName: "",
        contactPerson: "",
        contactPersonPhone: "",
        street: "",
        city: "",
        province: "",
        zipCode: "",
        country: "",
        TIN: "",
        businessType: null,
        paymentTerms: "",
        paymentMethod: null,
        productServicesProvided: "",
        contractStart: "",
        contractEnd: "",
        billingAddress1: "",
        billingAddress2: "",
        rating: "",
        notes: "",
        dateCreated: "",
      };
    },

    setVendorDetailsToInput(item) {
      this.customerData.firstName = item.firstName;
      this.customerData.middleName = item.middleName;
      this.customerData.lastName = item.lastName;
      this.customerData.contactPerson = item.contactPerson;
      this.customerData.contactPersonPhone = item.contactPersonPhone;
      this.customerData.street = item.street;
      this.customerData.city = item.city;
      this.customerData.province = item.province;
      this.customerData.zipCode = item.zipCode;
      this.customerData.country = item.country;
      this.customerData.TIN = item.TIN;
      this.customerData.businessType = item.businessType;
      this.customerData.paymentTerms = item.paymentTerms;
      this.customerData.paymentMethod = item.paymentMethod;
      this.customerData.productServicesProvided = item.productServicesProvided;
      this.customerData.contractStart = item.contractStart;
      this.customerData.contractEnd = item.contractEnd;
      this.customerData.billingAddress1 = item.billingAddress1;
      this.customerData.billingAddress2 = item.billingAddress2;
      this.customerData.rating = item.rating;
      this.customerData.notes = item.notes;
      this.customerData.dateCreated = item.dateCreated;
    },
  },
};
</script>
<style lang="sass" scoped>
</style>