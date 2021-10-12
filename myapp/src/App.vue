<template>
  <v-app>
    <!-- <v-app-bar app color="primary" dark> -->
    <!-- <Header /> -->
    <!-- </v-app-bar> -->

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
    <!-- <v-footer><Footer /> </v-footer> -->
  </v-app>
</template>

<script>
import axios from "axios";
// import Header from "./components/Header.vue";
import SearchPerson from "./components/SearchPerson.vue";
import AddPerson from "./components/AddEditPerson.vue";
import Persons from "./components/Persons.vue";
// import Footer from "./components/Footer.vue";

export default {
  name: "App",

  components: {
    // Header,
    SearchPerson,
    Persons,
    AddPerson,
    // Footer,
  },

  data() {
    return {
      persons: [],
      status: "load",
      inputSearchFName: "",
      inputSearchLName: "",
      inputSearchEmail: "",
    };
  },

  methods: {
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
    async addPerson(person) {
      try {
        await axios.post("http://localhost:3000/persons/", person);
        this.loadPersons();
      } catch (error) {
        console.log(error);
      }
    },
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
    handleFName(val) {
      this.inputSearchFName = val;
    },
    handleLName(val) {
      this.inputSearchLName = val;
    },
    handleEmail(val) {
      this.inputSearchEmail = val;
    },
  },
};
</script>

<style>
.v-application {
  background-image: url("./assets/back.jpg") !important;
  height: 100% !important;
  background-position: center !important;
  background-repeat: no-repeat !important;
  background-size: contain !important;
  background-attachment: fixed !important;
}

.delete-edit-btns {
  display: flex;
  justify-content: flex-start;
  align-content: center;
}
</style>
