<template>
  <v-card elevation="2" outlined class="person-card">
    <p><strong>FirstName : </strong> {{ person.firstName }}</p>
    <p><strong>LastName : </strong>{{ person.lastName }}</p>
    <p><strong>Email : </strong> {{ person.email }}</p>
    <!-- delete button  -->
    <div class="delete-edit-btns">
      <img
        src="../assets/delete.png"
        alt="delete person"
        width="60px"
        @click="() => deletePerson(person.id)"
      />
      <!-- add button with modal imported as EditPerson name  -->
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
};
</script>

<style>
.person-card {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 550px;
  padding: 20px;
  margin: 20px;
  background-color: #fafbfc !important;
  color: rgb(105, 105, 105) !important;
}
.delete-edit-btns img {
  width: 60px;
  cursor: pointer;
  margin: 0 20px;
}
</style>
