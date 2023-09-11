<template>
  <!--  Title Page -->
  <v-container fluid>
    <h2 class="text-grey pa-2">
      <v-icon size="small">mdi-account</v-icon> Team
    </h2>
  </v-container>
  <!-- End Title Page -->

  <!-- Content Page -->
  <v-container>
    <v-row>
      <v-col cols="12" class="my-3">
        <a href="https://employee-system-crud.netlify.app" target="_blank">
          <v-btn color="success">
            <v-icon>mdi-plus</v-icon> Add Employee
          </v-btn></a
        >
      </v-col>
    </v-row>
    <v-row>
      <v-col v-for="team in teams" :key="team" cols="12" sm="6" md="4">
        <v-card class="mx-auto pa-3 text-center" max-width="600">
          <v-avatar class="" size="140" rounded="50">
            <v-img cover :src="team.avatar"></v-img>
          </v-avatar>

          <v-card-text>
            <div style="font-size: 18px; text-transform: capitalize">
              {{ team.fullname }}
            </div>
            <div class="text-grey my-2" style="text-transform: capitalize">
              {{ team.dept }}
            </div>
            <div class="text-grey my-2">{{ team.email }}</div>
          </v-card-text>

          <v-card-actions>
            <v-btn variant="outlined" color="grey">
              <v-icon size="small">mdi-message</v-icon> Message
            </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
  <!-- End Content Page -->
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      teams: [],
    };
  },
  async mounted() {
        window.scrollTo(0, 0);
    await axios
      .get(
        "https://employee-crud-e1e7a-default-rtdb.firebaseio.com/users/employee.json"
      )
      .then((res) => {
        this.teams = res.data;
      });
  },
};
</script>

<style scoped>
a {
  text-decoration: none;
}
</style>
