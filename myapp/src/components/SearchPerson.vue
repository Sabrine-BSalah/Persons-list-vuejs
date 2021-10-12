<template>
  <v-form>
    <v-container fluid>
      <v-row class="search-bar">
        <v-col cols="12" sm="6" md="4">
          <!-- Search by firstname input -->
          <v-text-field @input="handleFname">
            <template v-slot:label>
              Search by <strong>firstName</strong> here?
              <v-icon style="vertical-align: middle">
                mdi-file-find
              </v-icon>
            </template>
          </v-text-field>
        </v-col>
        <!-- Search by lastname input -->
        <v-col cols="12" sm="6" md="4">
          <v-text-field @input="handleLname">
            <template v-slot:label>
              Search by <strong>lastname</strong> here?
              <v-icon style="vertical-align: middle">
                mdi-file-find
              </v-icon>
            </template>
          </v-text-field>
        </v-col>
        <!-- Search by email input -->
        <v-col cols="12" sm="6" md="4">
          <v-text-field @input="handleMail">
            <template v-slot:label>
              Search by <strong>email</strong> here?
              <v-icon style="vertical-align: middle">
                mdi-file-find
              </v-icon>
            </template>
          </v-text-field>
        </v-col>
      </v-row>
    </v-container>
  </v-form>
</template>

<script>
export default {
  name: "SearchPerson",

  methods: {
    // send value of input firstname value to the parent
    handleFname(e) {
      this.$emit("handle-fname", e);
    },
    // send value of input lastname value to the parent
    handleLname(e) {
      this.$emit("handle-lname", e);
    },
    // send value of input email value to the parent
    handleMail(e) {
      this.$emit("handle-email", e);
    },
  },
};
</script>

<style>
.search-bar {
  background-color: #eaf3fa;
  border-radius: 20px;
  border: 2px solid #e5f2fc;
  margin: 0 15% !important;
  padding: 0 5%;
}
</style>
