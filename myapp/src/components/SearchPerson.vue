<template>
  <v-form>
    <v-container fluid>
      <v-row>
        <v-col cols="12" sm="6" md="4">
          <v-text-field @input="handleFname">
            <template v-slot:label>
              Search by <strong>firstName</strong> here?
              <v-icon style="vertical-align: middle">
                mdi-file-find
              </v-icon>
            </template>
          </v-text-field>
        </v-col>
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

  data() {
    return {};
  },
  methods: {
    handleFname(e) {
      this.$emit("handle-fname", e);
    },
    handleLname(e) {
      this.$emit("handle-lname", e);
    },
    handleMail(e) {
      this.$emit("handle-email", e);
    },
  },
};
</script>
