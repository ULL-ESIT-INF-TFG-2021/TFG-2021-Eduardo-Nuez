<template>
  <div class="q-px-xl column full-width ">
    <span class="text-h6 text-center text-light-green">DIRECCIÓN</span>
    <q-separator color="grey-4" />
    <div class="q-pt-lg row items-center">
      <span class="col-2 text-subtitle2 text-bold">Primera dirección
        <span class="text-red">*</span>
      </span>
      <q-input
        v-model="enterpriseFirstAddress"
        outlined
        class="q-pl-xl col-8"
        label="Primera dirección"
      />
    </div>
    <div class="q-pt-lg row items-center">
      <span class="col-2 text-subtitle2">Segunda dirección</span>
      <q-input
        v-model="enterpriseSecondAddress"
        outlined
        class="q-pl-xl col-8"
        label="Segunda dirección"
      />
    </div>
    <div class="q-pt-lg row items-center">
      <span class="col-2 text-subtitle2 text-bold">Ciudad / Código postal
        <span class="text-red">*</span>
      </span>
      <q-input
        v-model="enterpriseProvince"
        outlined
        class="q-pl-xl col-4"
        label="Ciudad"
      />
      <q-input
        v-model="enterpriseZipCode"
        outlined
        class="q-pl-md col-4"
        label="ZIP"
      />
    </div>
    <div class="q-pt-lg row items-center">
      <span class="col-2 text-subtitle2 text-bold">País
        <span class="text-red">*</span>
      </span>
      <q-input
        v-model="enterpriseCountry"
        outlined
        class="q-pl-xl col-8"
        label="País"
      />
    </div>
    <div class="row justify-end">
      <q-btn
        color="secondary"
        label="Actualizar campos"
        class="full-height q-mt-xl"
        @click="emitChanges"
      />
    </div>
  </div>
</template>

<script>
export default {
  name: "Address",
  props: {
    country: {
      type: String,
      default: ""
    },
    firstAddress: {
      type: String,
      default: ""
    },
    zipCode: {
      type: String,
      default: ""
    },
    province: {
      type: String,
      default: ""
    },
    secondAddress: {
      type: String,
      default: ""
    }
  },
  data () {
    return {
      enterpriseCountry: this.country,
      enterpriseFirstAddress: this.firstAddress,
      enterpriseZipCode: this.zipCode,
      enterpriseProvince: this.province,
      enterpriseSecondAddress: this.secondAddress
    }
  },
  methods: {
    emitChanges () {
      this.$emit("update:country", this.enterpriseCountry)
      this.$emit("update:firstAddress", this.enterpriseFirstAddress)
      this.$emit("update:zipCode", this.enterpriseZipCode)
      this.$emit("update:secondAddress", this.enterpriseSecondAddress)
      this.$emit("update:province", this.enterpriseProvince)
      this.$emit("clickUpdateEnterprise")
    }
  }
}
</script>
