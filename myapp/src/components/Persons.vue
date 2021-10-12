<template>
  <v-container fluid class="list-persons">
    <!-- <v-row class="mt-4"> -->
    <p v-if="status == 'error'">{{ error }}</p>
    <div v-else-if="status == 'ready'">
      <div v-for="person in persons" v-bind:key="person.id">
        <Person
          :person="person"
          :deletePerson="deletePerson"
          :editPerson="editPerson"
        />
      </div>
    </div>
    <p v-else-if="status == 'load'">
      Loading Persons ...
      <Loading />
    </p>
    <!-- </v-row> -->
  </v-container>
</template>

<script>
import axios from "axios";
import Loading from "./Loading.vue";
import Person from "./Person.vue";

export default {
  name: "Persons",

  components: {
    Loading,
    Person,
  },

  data() {
    return {
      error: "",
      edit: false,
    };
  },

  props: ["persons", "loadPersons", "status"],

  mounted() {
    this.loadPersons();
  },

  methods: {
    async deletePerson(personId) {
      try {
        confirm("Are you sure to delete Person ?") &&
          (await axios.delete(`http://localhost:3000/persons/${personId}`));
        this.loadPersons();
      } catch (error) {
        console.log(error);
      }
    },
    async editPerson(personId, newPerson) {
      try {
        await axios.put(`http://localhost:3000/persons/${personId}`, newPerson);
        this.loadPersons();
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style>
.list-persons {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
</style>
