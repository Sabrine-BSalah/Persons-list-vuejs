<template>
  <v-card elevation="2" outlined class="person-card">
    <h2>FirstName : {{ person.firstName }}</h2>
    <h2>LastName :{{ person.lastName }}</h2>
    <h2>Email : {{ person.email }}</h2>
    <div class="delete-edit-btns">
      <v-btn
        x-large
        color="success"
        dark
        @click="() => deletePerson(person.id)"
      >
        Delete
      </v-btn>
      <EditPerson edit="true" :editPerson="editPerson" :person="person" />
    </div>
  </v-card>
</template>

<script>
import EditPerson from "./AddEditPerson.vue";

export default {
  name: "Person",

  components: {
    EditPerson,
  },
  props: ["person", "deletePerson", "editPerson"],

  methods: {},
};
</script>

<style>
.person-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 800px;
  padding: 20px;
  margin: 20px;
}
</style>
