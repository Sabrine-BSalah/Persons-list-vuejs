<template>
  <v-dialog v-model="dialog" persistent max-width="600px">
    <!-- depending on the value of boolean edit we display neither edit button or delete button  -->
    <template v-slot:activator="{ on, attrs }">
      <!-- if edit true display edit button -->
      <v-img
        v-if="edit"
        src="../assets/edit.png"
        v-bind="attrs"
        v-on="on"
        alt="edit"
        width="60px"
      />
      <!-- else display add button -->
      <v-btn
        v-else
        x-large
        v-bind="attrs"
        v-on="on"
        center
        class="grey lighten-4"
      >
        Add
      </v-btn>
    </template>
    <!-- details of modal when opened conditional rendering  according to edit value-->
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
        <!--  close button -->
        <v-btn color="blue darken-1" text @click="dialog = false">
          Close
        </v-btn>
        <!--  add or edit button -->
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
  // Validations errors of inputs
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
    //  Handling errors firstname before add or edit
    handleErrorsFirstName() {
      const errors = [];
      if (!this.$v.firstName.$dirty) return errors;
      !this.$v.firstName.maxLength &&
        errors.push("FirstName must have at most 15 characters !! ");
      !this.$v.firstName.required && errors.push("FirstName is required !!");
      return errors;
    },
    //  Handling errors lastname before add or edit
    handleErrorsLastName() {
      const errors = [];
      if (!this.$v.lastName.$dirty) return errors;
      !this.$v.lastName.maxLength &&
        errors.push("LastName must have at most 15 characters !!");
      !this.$v.lastName.required && errors.push("lastName is required !!");
      return errors;
    },
    //  Handling errors email before add or edit
    handleErrorsEmail() {
      const errors = [];
      if (!this.$v.email.$dirty) return errors;
      !this.$v.email.email && errors.push("Enter a valid email !!");
      !this.$v.email.required && errors.push("Email is required !!");
      return errors;
    },
  },

  methods: {
    // close modal method
    closeModal() {
      this.dialog = false;
    },
    // Initialize inputs to use after add new person
    cleanInputs() {
      this.firstName = "";
      this.lastName = "";
      this.email = "";
      this.$v.$reset();
    },
    // Method with condition if edit true==> edit a person, else ==>add new person
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
