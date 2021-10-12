<template>
  <v-app>
    <v-main class="main-app">
      <div>
        <SearchPerson
          v-on:handle-fname="handleFName"
          v-on:handle-lname="handleLName"
          v-on:handle-email="handleEmail"
        />
        <AddPerson :addPerson="addPerson" />
      </div>
      <Persons
        :persons="
          filterByFirstName(
            inputSearchFName,
            inputSearchLName,
            inputSearchEmail
          )
        "
        :loadPersons="loadPersons"
        :status="status"
        :inputSearchFName="inputSearchFName"
        :inputSearchLName="inputSearchLName"
        :inputSearchEmail="inputSearchEmail"
      />
    </v-main>
  </v-app>
</template>

<script>
import axios from "axios";
import SearchPerson from "./components/SearchPerson.vue";
import AddPerson from "./components/AddEditPerson.vue";
import Persons from "./components/Persons.vue";

export default {
  name: "App",

  components: {
    SearchPerson,
    Persons,
    AddPerson,
  },

  data() {
    return {
      persons: [],
      status: "load", // load when waiting the list of persons
      inputSearchFName: "",
      inputSearchLName: "",
      inputSearchEmail: "",
    };
  },

  methods: {
    // Fetch list of persons and change the value of "status" which control if success or fail
    async loadPersons() {
      try {
        const p = await axios.get("http://localhost:3000/persons");
        this.persons = await p.data;
        this.status = "ready";
      } catch (error) {
        this.error = error;
        this.status = "error";
      }
    },
    // Add request method and call of loadPersons() to fetch after add new person
    async addPerson(person) {
      try {
        await axios.post("http://localhost:3000/persons/", person);
        this.loadPersons();
      } catch (error) {
        console.log(error);
      }
    },
    // Filter list of persons with firstname && lastname && email
    filterByFirstName(inputSearchFName, inputSearchLName, inputSearchEmail) {
      return this.persons.filter(
        (person) =>
          person.firstName
            .toUpperCase()
            .includes(inputSearchFName.toUpperCase()) &&
          person.lastName
            .toUpperCase()
            .includes(inputSearchLName.toUpperCase()) &&
          person.email.toUpperCase().includes(inputSearchEmail.toUpperCase())
      );
    },
    // handle firstname method
    handleFName(val) {
      this.inputSearchFName = val;
    },
    // handle lastname method
    handleLName(val) {
      this.inputSearchLName = val;
    },
    // handle email method
    handleEmail(val) {
      this.inputSearchEmail = val;
    },
  },
};
</script>

<style>
.v-application {
  background-image: url("./assets/back2.jpg") !important;
  height: 100% !important;
  background-position: center !important;
  background-repeat: no-repeat !important;
  background-size: contain !important;
  background-attachment: fixed !important;
  text-align: center;
}

.delete-edit-btns {
  display: flex;
  justify-content: flex-start;
  align-content: center;
}
</style>
