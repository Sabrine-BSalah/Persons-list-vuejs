<template>
  <v-container fluid class="list-persons">
    <!-- if can not get list of users display the error -->
    <p v-if="status == 'error'">{{ error }}</p>

    <!-- if we have the list of persons with success, map the list, each element in a component Person  -->
    <div v-else-if="status == 'ready'">
      <div v-for="person in persons" v-bind:key="person.id">
        <Person
          :person="person"
          :deletePerson="deletePerson"
          :editPerson="editPerson"
        />
      </div>
    </div>

    <!-- if we wait the list of persons load  -->
    <p v-else-if="status == 'load'">
      <Loading />
    </p>
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
      edit: false, // to use in modal double functionnality add and edit person
    };
  },

  props: ["persons", "loadPersons", "status"],

  mounted() {
    this.loadPersons();
  },

  methods: {
    // add person method
    async deletePerson(personId) {
      try {
        confirm("Are you sure to delete Person ?") &&
          (await axios.delete(`http://localhost:3000/persons/${personId}`));
        this.loadPersons();
      } catch (error) {
        console.log(error);
      }
    },
    // edit person method
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
