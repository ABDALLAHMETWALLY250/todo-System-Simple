<template>
  <v-container>
    <v-form class="w-100" ref="form">
      <v-row class="w-100">
        <v-col
          cols="12"
          sm="6"
          :class="{ 'form-error': v$.dataProject.title.$error }"
          md="4"
        >
          <v-text-field
            label="Title"
            v-model="dataProject.title"
            :rules="inputRules"
            required
            style="text-transforn: capitalize"
          ></v-text-field>
          <span class="error-feedback" v-if="v$.dataProject.title.$error">{{
            v$.dataProject.title.$errors[0].$message
          }}</span>
        </v-col>

        <v-col
          :class="{ 'form-error': v$.dataProject.person.$error }"
          cols="12"
          sm="6"
          md="4"
        >
          <v-text-field
            label="Person"
            v-model="dataProject.person"
            :rules="inputRules"
            style="text-transform: capitalize"
            required
          ></v-text-field>
          <span class="error-feedback" v-if="v$.dataProject.person.$error">
            {{ v$.dataProject.person.$errors[0].$message }}
          </span>
        </v-col>

        <v-col
          :class="{ 'form-error': v$.dataProject.status.$error }"
          cols="12"
          sm="6"
          md="4"
        >
          <v-text-field
            label="Status (complete , overdue , ongoing)"
            v-model="dataProject.status"
            :rules="inputRules"
            required
            style="text-transform: lowercase"
          ></v-text-field>
          <span class="error-feedback" v-if="v$.dataProject.status.$error">
            {{ v$.dataProject.status.$errors[0].$message }}
          </span>
        </v-col>

        <v-col
          :class="{ 'form-error': v$.dataProject.avatar.$error }"
          cols="12"
          sm="6"
          md="12"
        >
          <v-text-field
            label="Ypur Image"
            v-model="dataProject.avatar"
            :rules="inputRules"
            required
          ></v-text-field>
          <span class="error-feedback" v-if="v$.dataProject.avatar.$error">
            {{ v$.dataProject.avatar.$errors[0].$message }}
          </span>
        </v-col>

        <v-col
          :class="{ 'form-error': v$.dataProject.content.$error }"
          cols="12"
          sm="12"
          md="12"
        >
          <v-textarea
            label="Information"
            v-model="dataProject.content"
            style="text-transform: capitalize"
            :rules="inputRules"
          ></v-textarea>
          <span class="error-feedback" v-if="v$.dataProject.content.$error">
            {{ v$.dataProject.content.$errors[0].$message }}
          </span>
        </v-col>

        <v-col cols="12" sm="12" md="12">
          <v-menu
            v-model="menu1"
            :close-on-content-click="false"
            max-width="290"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-text-field
                :value="computedDateFormattedMomentjs"
                clearable
                label="Date"
                readonly
                v-bind="attrs"
                v-on="on"
                @click:clear="due = null"
              ></v-text-field>
            </template>
            <v-date-picker
              v-model="due"
              @change="menu1 = false"
            ></v-date-picker>
          </v-menu>
        </v-col>
      </v-row>
    </v-form>
    <v-btn class="me-2" color="success" @click="sumbit">
      <v-icon>mdi-plus</v-icon> Add</v-btn
    >
    <v-btn
      class="me-2"
      @click="$router.push('/')"
      variant="outlined"
      color="red"
    >
      <v-icon>mdi-close</v-icon> Cancel</v-btn
    >
  </v-container>
</template>
<script>
import moment from "moment";
import { format, parseISO } from "date-fns";
import axios from "axios";
import { useVuelidate } from "@vuelidate/core";
import { required } from "@vuelidate/validators";
export default {
  data: () => ({
    menu1: false,
    menu2: false,
    dialog: false,
    loading: false,
    v$: useVuelidate(),
    dataProject: {
      title: "",
      person: "",
      status: "",
      avatar: "",
      content: "",
      due: format(parseISO(new Date().toISOString()), "yyyy-MM-dd"),
    },
    //inputRules: [(v) => v.length >= 3 || "Minimum length is 3 charactrs"],
  }),
  validations() {
    return {
      dataProject: {
        title: { required },
        person: { required },
        status: { required },
        avatar: { required },
        content: { required },
      },
    };
  },
  methods: {
    sumbit() {
      this.v$.dataProject.$validate();
      if (!this.v$.dataProject.$error) {
        this.loading = true;
        axios
          .post(
            "https://todo-leader-default-rtdb.firebaseio.com/project.json",
            this.dataProject
          )
          .then((res) => {
            this.dialog = false;
            this.loading = false;
            this.$emit("projectAdded");
            this.$router.push("/");
          });
      }
    },
  },
  computed: {
    computedDateFormattedMomentjs() {
      return this.dataProject.due
        ? moment(this.due).format("dddd, MMMM Do YYYY")
        : "";
    },
    computedDateFormattedDatefns() {
      return this.dataProject.due
        ? format(parseISO(this.due), "EEEE, MMMM do yyyy")
        : "";
    },
  },
};
</script>

<style lang="scss" scoped>
.form-error {
  color: rgb(193, 3, 3);
}
</style>
