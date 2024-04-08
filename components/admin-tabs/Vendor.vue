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

      <b-button @click="addVendor()" variant="primary">
        <font-awesome-icon :icon="['fas', 'plus']" />
        Add vendor
      </b-button>
    </div>

    <b-table
      show-empty
      class="table-style"
      scrollable
      sticky-header
      no-border-collapse
      :fields="vendorFields"
      :items="vendorList"
      hover
    >
      <template v-slot:cell(vendorId)="row">
        {{ "VNDR" + String(row.item.vendorId).padStart(5, "0") }}
      </template>

      <template v-slot:cell(fullName)="row">
        {{ row.item.firstName + " " + row.item.lastName }}
      </template>

      <template v-slot:cell(viewDetails)="row">
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
      id="detailsModal"
      size="huge"
      header-bg-variant="primary"
      header-text-variant="light"
      body-bg-variant="gray"
      scrollable
      hide-footer
    >
      <template v-slot:modal-title>
        <h6>Vendor Details</h6>
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
              v-model="vendorData.firstName"
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
              v-model="vendorData.middleName"
            />
          </div>
          <div class="w-25 m-3">
            <small class="text-left">Last Name</small>
            <b-form-input
              :disabled="!isEditMode"
              type="text"
              :maxlength="10"
              class="form-text"
              v-model="vendorData.lastName"
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
              v-model="vendorData.contactPerson"
            />
          </div>
          <div class="w-25 m-3">
            <small class="text-left">Contact Person Phone</small>
            <b-form-input
              :disabled="!isEditMode"
              type="text"
              :maxlength="10"
              class="form-text"
              v-model="vendorData.contactPersonPhone"
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
              v-model="vendorData.street"
            />
          </div>
          <div class="w-25 m-3">
            <small class="text-left">City</small>
            <b-form-input
              :disabled="!isEditMode"
              type="text"
              :maxlength="10"
              class="form-text"
              v-model="vendorData.city"
            />
          </div>
          <div class="w-25 m-3">
            <small class="text-left">Province</small>
            <b-form-input
              :disabled="!isEditMode"
              type="text"
              :maxlength="10"
              class="form-text"
              v-model="vendorData.province"
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
              v-model="vendorData.zipCode"
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
              v-model="vendorData.TIN"
            />
          </div>
          <div class="w-25 m-3">
            <small class="text-left">Business Type</small>
            <b-form-select
              :disabled="!isEditMode"
              id="businessType"
              v-model="vendorData.businessType"
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
              v-model="vendorData.paymentTerms"
            />
          </div>
          <div class="w-25 m-3">
            <small class="text-left">Payment Method</small>
            <b-form-select
              :disabled="!isEditMode"
              id="businessType"
              v-model="vendorData.paymentMethod"
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
              v-model="vendorData.productServicesProvided"
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
              v-model="vendorData.contractStart"
            />
          </div>
          <div class="w-25 m-3">
            <small class="text-left">Contract End</small>
            <b-form-input
              :disabled="!isEditMode"
              type="date"
              class="form-text"
              v-model="vendorData.contractEnd"
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
              v-model="vendorData.billingAddress1"
            />
          </div>
          <div class="w-25 m-3">
            <small class="text-left">Billing address 2</small>
            <b-form-input
              :disabled="!isEditMode"
              type="text"
              :maxlength="10"
              class="form-text"
              v-model="vendorData.billingAddress2"
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
              v-model="vendorData.rating"
            />
          </div>
          <div class="w-25 m-3">
            <small class="text-left">Notes</small>
            <b-form-input
              :disabled="!isEditMode"
              type="text"
              :maxlength="10"
              class="form-text"
              v-model="vendorData.notes"
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
            <b-button class="w-100">Close</b-button>
          </div>
          <div class="w-25 m-3"></div>
        </div>
      </div>
    </b-modal>
  </div>
</template>
<script>
import { VENDOR_DATA, PAYMENT_METHODS } from "../../test/data/constants.js";
import { VENDOR } from "../../model/vendor.js";
export default {
  data() {
    return {
      vendorFields: [
        { key: "vendorId", label: "Vendor ID" },
        { key: "fullName", label: "Full Name" },
        { key: "contractStart", label: "Contract Start" },
        { key: "contractEnd", label: "Contract End" },
        { key: "dateCreated", label: "Date Created" },
        { key: "viewDetails", label: "View Details" },
      ],
      vendorList: VENDOR_DATA,
      vendorData: VENDOR,
      businessTypeOptions: [{ value: null, text: "Please select an option" }],
      paymentMethodOptions: PAYMENT_METHODS,
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
      this.vendorData = VENDOR;
      this.$bvModal.show("detailsModal");
      this.setVendorDetailsToInput(item);

      console.log(VENDOR);
    },

    addVendor() {
      this.isEditMode = true;
      this.isAdd = true;
      this.$bvModal.show("detailsModal");
      this.vendorData = {
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
      this.vendorData.firstName = item.firstName;
      this.vendorData.middleName = item.middleName;
      this.vendorData.lastName = item.lastName;
      this.vendorData.contactPerson = item.contactPerson;
      this.vendorData.contactPersonPhone = item.contactPersonPhone;
      this.vendorData.street = item.street;
      this.vendorData.city = item.city;
      this.vendorData.province = item.province;
      this.vendorData.zipCode = item.zipCode;
      this.vendorData.country = item.country;
      this.vendorData.TIN = item.TIN;
      this.vendorData.businessType = item.businessType;
      this.vendorData.paymentTerms = item.paymentTerms;
      this.vendorData.paymentMethod = item.paymentMethod;
      this.vendorData.productServicesProvided = item.productServicesProvided;
      this.vendorData.contractStart = item.contractStart;
      this.vendorData.contractEnd = item.contractEnd;
      this.vendorData.billingAddress1 = item.billingAddress1;
      this.vendorData.billingAddress2 = item.billingAddress2;
      this.vendorData.rating = item.rating;
      this.vendorData.notes = item.notes;
      this.vendorData.dateCreated = item.dateCreated;
    },
  },
};
</script>
<style lang="sass" scoped>
</style>