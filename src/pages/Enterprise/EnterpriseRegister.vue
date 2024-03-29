<template>
  <q-page class="q-pa-md flex justify-center items-center">
    <div style="width: 50%;">
      <q-stepper
        ref="stepper"
        v-model="step"
        class="shadow-8"
        animated
        done-color="red-5"
        active-color="purple"
        inactive-color="secondary"
      >
        <q-step
          :name="1"
          title="Detalles"
          icon="manage_accounts"
          :done="step > 1"
        >
          <EnterpriseInformation
            :name.sync="name"
            :address1.sync="address1"
            :address2.sync="address2"
            :province.sync="province"
            :zip.sync="zip"
            :country.sync="country"
          />
        </q-step>

        <q-step
          :name="2"
          title="Contacto"
          icon="create_new_folder"
          :done="step > 2"
        >
          <EnterpriseContact
            :contact.sync="contact"
            :email.sync="email"
            :phone.sync="phone"
          />
        </q-step>

        <q-step
          :name="3"
          title="Tipo empresa"
          icon="add_comment"
          :done="step > 3"
        >
          <EnterpriseType :is-productor.sync="isProductor" />
        </q-step>
        <q-step
          :name="4"
          title="About"
          icon="manage_accounts"
          :done="step > 4"
        >
          <EnterpriseDetails
            :company-number.sync="companyNumber"
            :long-description.sync="longDescription"
            :short-description.sync="shortDescription"
          />
        </q-step>
        <q-step
          :name="5"
          title="Imagenes"
          icon="manage_accounts"
          :done="step > 5"
        >
          <EnterpriseImages
            :name="name"
            :owner="contact"
          />
        </q-step>
        <q-step
          :name="6"
          title="Redes sociales"
          icon="add_comment"
          :done="step > 6"
        >
          <EnterpriseSocial
            :name="name"
            :owner="contact"
            :web.sync="web"
            :twitter.sync="twitter"
            :facebook.sync="facebook"
            :instagram.sync="instagram"
            :linkedin.sync="linkedin"
          />
        </q-step>
        <template #navigation>
          <q-stepper-navigation>
            <q-btn
              color="deep-orange"
              :label="step === 6 ? 'Finish' : 'Continue'"
              @click="evaluateStepper()"
            />
            <q-btn
              v-if="step > 1"
              flat
              color="deep-orange"
              label="Back"
              class="q-ml-sm"
              @click="$refs.stepper.previous()"
            />
          </q-stepper-navigation>
        </template>
      </q-stepper>
    </div>
  </q-page>
</template>

<script>
import {
  EnterpriseInformation,
  EnterpriseContact,
  EnterpriseType,
  EnterpriseDetails,
  EnterpriseSocial,
  EnterpriseImages
} from "components/EnterpriseRegister/"

export default {
  name: "EnterpriseRegister",
  components: {
    EnterpriseImages,
    EnterpriseInformation,
    EnterpriseContact,
    EnterpriseType,
    EnterpriseDetails,
    EnterpriseSocial
  },
  data () {
    return {
      step: 1,
      imageData: null,
      name: "",
      address1: "",
      address2: "",
      province: "",
      zip: "",
      country: "",
      contact: "",
      email: "",
      phone: "",
      isProductor: "",

      longDescription: "",
      shortDescription: "",
      companyNumber: "",

      web: "",
      twitter: "",
      facebook: "",
      instagram: "",
      linkedin: ""

    }
  },
  computed: {
    generateEnterpriseObject () {
      return {
        name: this.name,

        address: {
          first_address: this.address1,
          second_address: this.address2,
          province: this.province,
          zip_code: this.zip,
          country: this.country
        },

        owner: this.contact,
        email: this.email,
        contact_phone: Number(this.phone),
        user_id: this.$store.getters["User/getUserId"],
        is_productor: this.isProductor === "productor"
      }
    },
    createEnterpriseDetails () {
      return {
        owner: this.contact,
        name: this.name,

        company_number: this.companyNumber,
        short_description: this.shortDescription,
        long_description: this.longDescription
      }
    },
    createEnterpriseSocial () {
      return {
        owner: this.contact,
        name: this.name,

        website: this.web,
        facebook: this.facebook,
        twitter: this.twitter,
        linkedin: this.linkedin,
        instagram: this.instagram
      }
    }
  },
  mounted () {
    if (!this.$store.getters["User/getRole"]) {
      this.$router.push("/user/login")
    }
  },
  methods: {
    setProductor (val) {
      this.isProductor = val
    },
    async evaluateStepper () {
      if (this.step === 1) {
        if (this.name !== "" &&
        this.address1 !== "" &&
        this.province !== "" &&
        this.zip !== "" &&
        this.country !== ""
        ) this.$refs.stepper.next()
      } else if (this.step === 2) {
        if (this.contact !== "" &&
        this.email !== "" &&
        this.phone !== "") this.$refs.stepper.next()
      } else if (this.step === 3) {
        if (this.isProductor !== "") {
          this.createNewEnterprise()
          this.$refs.stepper.next()
        }
      } else if (this.step === 4) {
        if (this.longDescription !== "" ||
            this.shortDescription !== "" ||
            this.companyNumber !== "") {
          this.notification(await this.$store.dispatch("EnterpriseRegister/updateEnterpriseDetails", this.createEnterpriseDetails))
        }
        this.$refs.stepper.next()
      } else if (this.step === 6) {
        if (this.web !== "" ||
            this.linkedin !== "" ||
            this.instagram !== "" ||
            this.twitter !== "" ||
            this.facebook !== "") {
          this.notification(await this.$store.dispatch("EnterpriseRegister/updateEnterpriseSocial", this.createEnterpriseSocial))
          this.$router.push("/enterprise/")
        }
      } else {
        this.$refs.stepper.next()
      }
    },
    async createNewEnterprise () {
      this.notification(await this.$store.dispatch("EnterpriseRegister/newEnterprise", this.generateEnterpriseObject))
    },
    notification  (res) {
      if (res.data.success) {
        this.$q.notify({
          message: res.data.msg,
          type: "positive",
          position: "top"
        })
      } else {
        this.$q.notify({
          message: res.data.error,
          type: "negative",
          position: "top"
        })
      }
    }
  }
}
</script>
