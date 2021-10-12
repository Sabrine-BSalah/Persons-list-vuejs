<template>
  <v-dialog v-model="dialog" persistent max-width="600px">
    <template v-slot:activator="{ on, attrs }">
      <v-btn color="primary" dark v-bind="attrs" v-on="on" x-large>
        {{ edit ? "Edit Person" : "Add Person" }}
      </v-btn>
    </template>
    <v-card>
      <v-card-title>
        <span class="text-h5">{{
          edit ? "Edit Person" : "Add new Person"
        }}</span>
      </v-card-title>
      <v-card-text>
        <v-container>
          <v-row>
            <!-- FirstName -->
            <v-col cols="12" sm="6">
              <v-text-field
                label="First name*"
                :error-messages="handleErrorsFirstName"
                :counter="15"
                required
                @input="$v.firstName.$touch()"
                @blur="$v.firstName.$touch()"
                v-model="firstName"
              ></v-text-field>
            </v-col>

            <!-- LastName -->
            <v-col cols="12" sm="6">
              <v-text-field
                label="Last name*"
                persistent-hint
                required
                :error-messages="handleErrorsLastName"
                :counter="15"
                @input="$v.lastName.$touch()"
                @blur="$v.lastName.$touch()"
                v-model="lastName"
              ></v-text-field>
            </v-col>

            <!-- Email -->
            <v-col cols="12">
              <v-text-field
                label="Email*"
                required
                :error-messages="handleErrorsEmail"
                @input="$v.email.$touch()"
                @blur="$v.email.$touch()"
                v-model="email"
              ></v-text-field>
            </v-col>
          </v-row>
        </v-container>
        <small>*indicates required field</small>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn color="blue darken-1" text @click="dialog = false">
          Close
        </v-btn>
        <v-btn color="blue darken-1" text @click="addOrEditPerson">
          {{ edit ? "Edit" : "Add" }}
        </v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
import { validationMixin } from "vuelidate";
import { required, maxLength, email } from "vuelidate/lib/validators";

export default {
  mixins: [validationMixin],
  validations: {
    firstName: { required, maxLength: maxLength(15) },
    lastName: { required, maxLength: maxLength(15) },
    email: { required, email },
  },

  data() {
    return {
      dialog: false,
      firstName: "",
      lastName: "",
      email: "",
    };
  },

  props: ["edit", "addPerson", "editPerson", "person"],

  created() {
    this.firstName = this.edit ? this.person.firstName : "";
    this.lastName = this.edit ? this.person.lastName : "";
    this.email = this.edit ? this.person.email : "";
  },

  computed: {
    handleErrorsFirstName() {
      const errors = [];
      if (!this.$v.firstName.$dirty) return errors;
      !this.$v.firstName.maxLength &&
        errors.push("FirstName must have at most 15 characters !! ");
      !this.$v.firstName.required && errors.push("FirstName is required !!");
      return errors;
    },
    handleErrorsLastName() {
      const errors = [];
      if (!this.$v.lastName.$dirty) return errors;
      !this.$v.lastName.maxLength &&
        errors.push("LastName must have at most 15 characters !!");
      !this.$v.lastName.required && errors.push("lastName is required !!");
      return errors;
    },
    handleErrorsEmail() {
      const errors = [];
      if (!this.$v.email.$dirty) return errors;
      !this.$v.email.email && errors.push("Enter a valid email !!");
      !this.$v.email.required && errors.push("Email is required !!");
      return errors;
    },
  },

  methods: {
    closeModal() {
      this.dialog = false;
    },

    cleanInputs() {
      this.firstName = "";
      this.lastName = "";
      this.email = "";
      this.$v.$reset();
    },
    addOrEditPerson() {
      {
        if (this.firstName && this.lastName && this.email) {
          if (this.edit) {
            this.editPerson(this.person.id, {
              firstName: this.firstName,
              lastName: this.lastName,
              email: this.email,
            });
          } else {
            this.addPerson({
              firstName: this.firstName,
              lastName: this.lastName,
              email: this.email,
            });
            this.cleanInputs();
          }
          this.closeModal();
        }
      }
    },
  },
};
</script>
