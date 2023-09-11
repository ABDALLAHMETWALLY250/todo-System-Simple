<template>
  <h2 class="text-grey pa-3" style="text-transform: capitalize">
    <v-icon size="small">mdi-eye</v-icon> View Project For
    {{ $route.params.name }}
  </h2>

  <!-- Card -->
  <v-container fluid class="my-5">
    <v-row>
      <v-col cols="12" sm="12" md="5">
        <v-img
          :src="
            projects.avatar
              ? projects.avatar
              : 'https://img.freepik.com/free-vector/illustration-businessman_53876-5856.jpg?w=740&t=st=1694359541~exp=1694360141~hmac=2d1fd744638e61c1b74b12b56b8aed4d077625d1f4e7b306e829ca20be6263f3'
          "
          :lazy-src="projects.avatar"
          class="rounded-lg height"
        >
          <template v-slot:placeholder>
            <v-row class="fill-height ma-0" align="center" justify="center">
              <v-progress-circular
                indeterminate
                color="grey-lighten-5"
              ></v-progress-circular>
            </v-row>
          </template>
        </v-img>
      </v-col>

      <v-col cols="12" sm="12" md="7">
        <ul class="pa-4 mb-6">
          <li
            class="m-lg m-md m-sm text-center"
            style="margin-top: 1em; justify-content: center"
          >
            <h4>{{ projects.content }}</h4>
          </li>

          <li class="m-lg m-md m-sm">
            <h4 style="text-align: left">
              <v-icon>mdi-folder</v-icon> Project Name :
            </h4>
            <h3>{{ projects.title }}</h3>
          </li>

          <li class="m-lg m-md m-sm">
            <h4 style="text-align: left">
              <v-icon>mdi-account</v-icon> Name :
            </h4>
            <h3>{{ projects.person }}</h3>
          </li>

          <li class="m-lg m-md m-sm">
            <h4 style="text-align: left">
              <v-icon>mdi-calendar</v-icon> Due :
            </h4>
            <h3>{{ projects.due }}</h3>
          </li>

          <li class="m-lg m-md m-sm">
            <h4 style="text-align: left">
              <v-icon>mdi-information</v-icon> Status :
            </h4>

            <div class="w-50">
              <h3 :class="`status ${projects.status ? projects.status : ''}`">
                {{ projects.status }}
              </h3>
            </div>
          </li>

          <li class="text-center">
            <v-btn
              class="me-3"
              @click="deleprojects(projects.id)"
              variant="outlined"
              color="red"
            >
              <v-icon>mdi-delete</v-icon>
              Delete
            </v-btn>
            <v-btn class="me-3" @click="$router.push('/')" color="blue">
              <v-icon>mdi-home</v-icon>
              Back
            </v-btn>
          </li>
        </ul>
      </v-col>
    </v-row>
  </v-container>
  <!-- End Card -->
  <!-- Loading Card 
  
  
  
  -->

  <!--End Loading Card  -->
</template>
<script>
import axios from "axios";
import { VSkeletonLoader } from "vuetify/labs/VSkeletonLoader";
export default {
  components: {
    VSkeletonLoader,
  },
  data() {
    return {
      projects: [],
    };
  },
  mounted() {
    this.getProject();
  },
  methods: {
    getProject() {
      axios
        .get(
          `https://todo-leader-default-rtdb.firebaseio.com/project/${this.$route.params.id}.json`
        )
        .then((res) => {
          this.projects = res.data;
        });
    },

    deleprojects(id) {
      axios
        .delete(
          `https://todo-leader-default-rtdb.firebaseio.com/project/${this.$route.params.id}.json`
        )
        .then((response) => {
          alert("This Project Is Deleted");
          this.$router.push("/");
        });
    },
  },
};
</script>

<style lang="scss" scoped>
.status.complete,
.status.Complete {
  padding: 5px 10px;
  border-radius: 10px;
  background-color: #3cd13c;
  width: fit-content;
  color: #fff;
}

.status.ongoing,
.status.Ongoing {
  padding: 5px 10px;
  border-radius: 10px;
  background-color: rgb(196, 133, 14);
  width: fit-content !important;
  color: #fff;
}
.status.overdue,
.status.Overdue {
  padding: 5px 10px;
  border-radius: 10px;
  background-color: rgb(228, 10, 10);
  width: fit-content;
  color: #fff;
}
@media screen and (max-width: 767px) {
  .m-sm {
    margin-top: 4em;
    margin-bottom: 4em;
    border-bottom: 1px solid;
    display: flex;
    align-items: center;
    justify-content: space-between;
    text-transform: capitalize;
  }
  .w-50 {
    text-align: -webkit-right;
  }
  .pa-4 {
    padding: 4px !important;
  }
}
@media screen and (min-width: 768px) and (max-width: 1023px) {
  .m-md {
    margin-top: 4em;
    border-bottom: 1px solid;
    margin-bottom: 4em;
    display: flex;
    align-items: center;
    justify-content: space-between;
    text-transform: capitalize;
  }
  .height {
    height: 70vh;
    width: 100%;
    object-fit: cover;
  }
  .w-50 {
    text-align: -webkit-right;
  }
}

@media screen and (min-width: 1024px) {
  .m-lg {
    margin-top: 4em;
    margin-bottom: 4em;
    border-bottom: 1px solid;
    display: flex;
    align-items: center;
    justify-content: space-between;
    text-transform: capitalize;
  }
  .height {
    height: 65vh;
    width: 100%;
    object-fit: cover;
  }
  .w-50 {
    text-align: -webkit-right;
  }
}
ul {
  list-style: none;
}
</style>
