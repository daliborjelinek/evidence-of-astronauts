<template>
  <div>
    <v-speed-dial
      fixed
      bottom
      right
      direction="top"
      open-on-hover
      transition="slide-y-reverse-transition"
    >
      <template v-slot:activator>
        <v-btn color="blue darken-2" dark fab>
          <v-avatar size="52px">
            <img src="../assets/face.jpg" />
          </v-avatar>
        </v-btn>
      </template>
      <v-btn
        target="_blank"
        href="https://github.com/daliborjelinek/"
        fab
        dark
        small
        color="#24292E"
      >
        <v-icon>mdi-github-face</v-icon>
      </v-btn>
      <v-btn
        target="_blank"
        href="https://www.linkedin.com/in/dalibor-jel%C3%ADnek-065038150/"
        fab
        dark
        small
        color="#0077B5"
      >
        <v-icon>mdi-linkedin</v-icon>
      </v-btn>

      <v-btn @click="displayCreditDialod = true" fab dark small color="#9FACBD">
        <v-icon>mdi-copyright</v-icon>
      </v-btn>
    </v-speed-dial>

    <v-dialog v-model="displayCreditDialod" max-width="500">
      <v-card color="#3C4856">
        <v-card-title class="headline">Credits</v-card-title>

        <v-card-text>
          <p>
            I've created this simple project to get more familiar with
            <a
              href="https://vuetifyjs.com/en/"
            >Vuetify</a> which really impressed me and to present my front-end skills in 2020.
          </p>
          <b>Thanks to</b>
          <ul>
            <li>
              Icon8 for
              <a
                href="https://icons8.com/icon/88825/astronaut-helmet"
              >Astronaut Helmet icon</a>.
            </li>
            <li>
              Vazha Omanashvili for awesome
              <a
                href="https://next.json-generator.com/"
              >Json generoator</a>.
            </li>
            <li>
              Tobias Ahlin for fancy CSS
              <a href="https://tobiasahlin.com/spinkit/">spinner</a>.
            </li>
          </ul>
          <br />
          <br />
          <p class="font-italic font-weight-light text-right">Dalibor Jelínek</p>
        </v-card-text>
      </v-card>
    </v-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      displayCreditDialod: false
    };
  }
};
</script>