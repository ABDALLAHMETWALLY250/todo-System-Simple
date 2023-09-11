<template>
  <!--  Title Page -->
  <v-container fluid>
    <h2 class="text-grey pa-2">
      <v-icon size="small">mdi-folder</v-icon> My Project
    </h2>
  </v-container>
  <!-- End Title Page -->
  <!--  Title Page -->
  <v-container>
    <v-expansion-panels variant="inset">
      <v-expansion-panel
        v-for="project in projects"
        :key="project"
        class="my-4"
        :title="project.title"
      >
        <v-expansion-panel-text class="text-grey"
          >Due by {{ project.due }}
        </v-expansion-panel-text>
        <v-expansion-panel-text>{{ project.content }} </v-expansion-panel-text>
      </v-expansion-panel>
    </v-expansion-panels>
  </v-container>
  <!-- End Title Page -->
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      projects: [],
    };
  },
  computed: {
    myProjects() {
      return this.projects.filter((pro) => {
        return pro.person == "Abdallah";
      });
    },
  },
  mounted() {
        window.scrollTo(0, 0);
    axios
      .get("https://todo-leader-default-rtdb.firebaseio.com/project.json")
      .then((res) => {
        this.projects = res.data;
      });
  },
};
</script>
