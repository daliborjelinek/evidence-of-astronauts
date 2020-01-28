<template>
  <v-app id="inspire">
    <!-- Lazy loaded backgroud image -->
    <div :class="[{'bg-image': true}, {'invisible': loading}]"></div>
    <!-- Loading layer -->
    <loading :class="[{'invisible': !loading}]" />
    <v-content>
      <!-- Credits -->
      <credits :class="[ {'credits' : true}, {'invisible': loading}]" />
      <!-- Astronauts card -->
      <v-container id="container" :class="[{'fill-height': true}, {'invisible': loading}]" fluid>
        <v-row align="center" justify="center">
          <v-col cols="12" sm="10" md="6">
            <v-card class="elevation-12">
              <v-toolbar color="light-blue accent-4" dark flat>
                <v-toolbar-title>
                  <img
                    class="astronaut-icon"
                    src="https://img.icons8.com/material-outlined/96/FFFFFF/astronaut-helmet.png"
                  />
                  Astronauts evidence
                </v-toolbar-title>
                <v-spacer />
                <v-tooltip right>
                  <template v-slot:activator="{ on }">
                    <v-btn icon large @click="createAstronaut" v-on="on">
                      <v-icon>add_box</v-icon>
                    </v-btn>
                  </template>
                  <span>New astronaut</span>
                </v-tooltip>
              </v-toolbar>
              <v-card-text>
                <!-- Astronauts data table -->
                <v-data-table
                  :headers="headers"
                  :items="items"
                  :items-per-page="5"
                  class="elevation-1"
                >
                  <template v-slot:item.action="{ item }">
                    <v-btn @click="openDialog(item)" x-small fab dark text icon>
                      <v-icon dark>edit</v-icon>
                    </v-btn>
                  </template>
                </v-data-table>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-content>

    <!--Profile editor dialog -->
    <v-dialog v-model="diplayProfileEditorDialog" max-width="290">
      <v-card>
        <v-card-title class="headline">Profile editor</v-card-title>

        <v-card-text>
          <v-container>
            <v-row>
              <v-col class="py-0" cols="12">
                <v-text-field
                  v-model="selectedItem.firstname"
                  autocomplete="none"
                  :counter="10"
                  label="First name"
                  required
                ></v-text-field>
              </v-col>

              <v-col class="py-0" cols="12">
                <v-text-field
                  v-model="selectedItem.lastname"
                  autocomplete="none"
                  :counter="10"
                  label="Last name"
                  required
                ></v-text-field>
              </v-col>

              <v-col class="py-0" cols="12">
                <v-text-field
                  v-model="selectedItem.superpower"
                  autocomplete="none"
                  :counter="10"
                  label="Superpower"
                  required
                ></v-text-field>
              </v-col>

              <v-col class="py-0" cols="12">
                <v-menu
                  v-model="diplayTimePicker"
                  transition="scale-transition"
                  offset-y
                  min-width="290px"
                >
                  <template v-slot:activator="{ on }">
                    <v-text-field
                      :value="selectedItem.birthdate"
                      label="Birthdate"
                      readonly
                      v-on="on"
                    ></v-text-field>
                  </template>
                  <v-date-picker
                    v-model="selectedItem.birthdate"
                    @input="diplayTimePicker = false"
                    no-title
                    scrollable
                  ></v-date-picker>
                </v-menu>
              </v-col>
            </v-row>
          </v-container>
        </v-card-text>

        <v-card-actions>
          <v-spacer></v-spacer>

          <v-btn color="red darken-1" text @click="deleteAstronaut()">DELETE</v-btn>
          <v-btn color="blue darken-1" text @click="diplayProfileEditorDialog = false">CONFIRM</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-app>
</template>

<script>
function Astronaut() {
  this.firstname = "";
  this.lastname = "";
  this.birthdate = "";
  this.superpower = "";
}
import Credits from "./components/Credits";
import Loading from "./components/Loading";
export default {
  name: "App",
  data: () => ({
    loading: true,
    diplayProfileEditorDialog: false,
    diplayTimePicker: false,
    selectedItem: new Astronaut(),
    background: new Image(),
    headers: [
      { text: "Firstname", value: "firstname" },
      { text: "Surname", value: "lastname" },
      { text: "Birthdate", value: "birthdate" },
      { text: "Superpower", value: "superpower" },
      { text: "Actions", value: "action", sortable: false, align: "cetnter" }
    ],
    items: []
  }),

  components: {
    Credits,
    Loading
  },
  mounted() {
    this.fetchBackground();
  },
  methods: {
    fetchBackground() {
      this.background.onload = this.fetchJson;
      this.background.src =
        "http://spacetrust.com/wp-content/uploads/2016/07/space01-min-min.jpg";
    },
    fetchJson() {
      fetch("https://next.json-generator.com/api/json/get/E17NlIOZd")
        .then(response => {
          return response.json();
        })
        .then(json => {
          this.items = json;
          this.loading = false;
        });
    },
    openDialog(item) {
      this.selectedItem = item;
      this.diplayProfileEditorDialog = true;
    },
    createAstronaut() {
      var newAstronaut = new Astronaut();
      this.items.push(newAstronaut);
      this.selectedItem = newAstronaut;
      this.diplayProfileEditorDialog = true;
    },
    deleteAstronaut() {
      let index = this.items.indexOf(this.selectedItem);
      this.items.splice(index, 1);
      this.diplayProfileEditorDialog = false;
    }
  }
};
</script>

<style>
.bg-image {
  background-attachment: fixed;
  background-size: cover;
  transition: all 3s ease-in-out;
  background-color: black;
  background-image: url("http://spacetrust.com/wp-content/uploads/2016/07/space01-min-min.jpg");
  width: 100%;
  height: 100%;
  position: fixed;
}

.astronaut-icon {
  display: inline-block;
  height: 1.5em;
  vertical-align: bottom;
}

.credits,
#container {
  transition: all 0.3s ease-in-out;
}

.invisible {
  opacity: 0;
}
</style>