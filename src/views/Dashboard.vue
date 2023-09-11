<template>
  <v-container fluid>
    <h2 class="text-grey pa-2">
      <v-icon size="small">mdi-view-dashboard</v-icon> Dashboard
    </h2>
  </v-container>

  <!-- Card -->
  <v-container fluid v-if="projects.length > 0" class="my-8">
    <v-row>
      <v-col v-for="project in projects" :key="project" cols="12" sm="6" md="4">
        <v-card
          :class="` mx-auto status ${
            project.status == 'complete' ||
            project.status == 'Complete' ||
            project.status == 'Ongoing' ||
            project.status == 'ongoing' ||
            project.status == 'Overdue' ||
            project.status == 'overdue'
              ? project.status
              : 'done'
          } `"
          max-width="800"
          elevation="2"
          variant="outlined"
        >
          <template v-slot:prepend>
            <v-card-title
              class="text-blue w-100"
              style="text-transform: capitalize; font-weight: bold"
            >
              <v-icon size="large">mdi-folder</v-icon>
              {{ project.title }}</v-card-title
            >
          </template>

          <v-card-text class="py-2 my-5 line-h">
            {{
              project.content.length > 190
                ? project.content.slice(0, 150) + "..."
                : project.content
            }}
          </v-card-text>

          <v-card-actions>
            <v-list-item class="w-100">
              <template v-slot:prepend>
                <v-avatar
                  color="grey-darken-3"
                  :image="
                    project.avatar
                      ? project.avatar
                      : 'https://img.freepik.com/free-vector/illustration-businessman_53876-5856.jpg?w=740&t=st=1694359541~exp=1694360141~hmac=2d1fd744638e61c1b74b12b56b8aed4d077625d1f4e7b306e829ca20be6263f3'
                  "
                ></v-avatar>
              </template>

              <v-list-item-title style="text-transform: capitalize">{{
                project.person
              }}</v-list-item-title>

              <v-list-item-subtitle>{{ project.due }}</v-list-item-subtitle>

              <template v-slot:append>
                <div class="d-flex align-center">
                  <v-icon
                    class="me-1"
                    color="red"
                    @click="deleProject(project.id)"
                    icon="mdi-delete"
                  ></v-icon>
                  <span class="me-1">·</span>
                  <v-icon
                    @click="
                      $router.push({
                        name: 'View Project',
                        params: {
                          id: project.id,
                          name: project.person,
                        },
                      })
                    "
                    class="me-1 text-blue"
                    icon="mdi-eye"
                  ></v-icon>
                  <span class="me-1">·</span>
                  <v-icon
                    @click="
                      $router.push({
                        name: 'Edit Project',
                        params: {
                          id: project.id,
                          name: project.person,
                        },
                      })
                    "
                    class="me-1 text-green"
                    icon="mdi-pencil"
                  ></v-icon>
                </div>
              </template>
            </v-list-item>
          </v-card-actions>
        </v-card>
      </v-col>

      <v-col cols="12">
        <v-btn color="success" @click="$router.push('/add-project')">
          <v-icon>mdi-plus</v-icon> Add A New Project
        </v-btn>
      </v-col>
    </v-row>
  </v-container>
  <!-- End Card -->
  <!-- Loading Card -->
  <v-container v-else fluid class="my-8">
    <v-row>
      <v-col v-for="n in 12" :key="n" cols="12" sm="6" md="4">
        <v-card>
          <v-skeleton-loader
            type="article,avatar,paragraph"
          ></v-skeleton-loader>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
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
        window.scrollTo(0, 0);
  },
  methods: {
    getProject() {
      axios
        .get("https://todo-leader-default-rtdb.firebaseio.com/project.json")
        .then((res) => {
          this.frameProjectData(res.data);
        });
    },
    frameProjectData(project) {
      this.projects = [];
      for (let key in project) {
        this.projects.push({ ...project[key], id: key });
      }
    },
    deleProject(id) {
      axios
        .delete(
          `https://todo-leader-default-rtdb.firebaseio.com/project/${id}.json`
        )
        .then((response) => {
          this.getProject();
        });
    },
  },
};
</script>

<style lang="scss" scoped>
.v-row {
  padding: 5px;
}
.v-row:hover {
  background-color: #fff;
  box-shadow: 0 5 5 8px gray;
}

.status.complete,
.status.Complete,
.status.done {
  border-left: 4px solid #3cd13c;
}

.status.ongoing,
.status.Ongoing {
  border-left: 4px solid rgb(196, 133, 14);
}
.status.overdue,
.status.Overdue {
  border-left: 4px solid rgb(228, 10, 10);
}
@media screen and (max-width: 767px) {
  .text-blue {
    display: flex;
    align-items: center;
    width: 67%;
  }
  .person {
    text-align: end;
  }

  .button-sm {
    display: flex !important;
  }
  .button-lg {
    display: none !important;
  }

  .center-sm {
    width: 100%;
    display: flex;
    align-items: center;
    justify-content: space-between;
    text-align: end;
    margin-bottom: 1em;
  }
  .sm-end {
    text-align: end !important;
    width: 100%;
  }
  .line-h {
    line-height: 24px !important;
  }
  .v-card-text.py-2 {
    font-weight: 600;
    font-size: 15px;
    ///زياده
    height: 15vh;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    ///زياده
  }
}
@media screen and (min-width: 768px) and (max-width: 1023px) {
  .person {
    text-align: center;
  }
  .button-sm {
    display: none !important;
  }
  .button-lg {
    display: flex !important;
  }
  .line-h {
    line-height: 28px !important;
  }
  .v-card-text.py-2 {
    font-weight: 600;
    font-size: 15px;
    ///زياده
    height: 16vh;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    padding-top: 0 !important;
    ///زياده
  }
}
@media screen and (min-width: 1024px) {
  .person {
    text-align: center;
  }
  .button-sm {
    display: none !important;
  }
  .button-lg {
    display: flex !important;
  }
  .line-h {
    line-height: 30px !important;
  }
  .v-card-text.py-2 {
    font-weight: 600;
    font-size: 16px;
    ///زياده
    height: 15vh;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
    ///زياده
  }
}
</style>
