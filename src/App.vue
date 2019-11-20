<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
          width="100"
        />
      </div>

      <v-spacer></v-spacer>
    </v-app-bar>

    <v-content>
      <v-simple-table>
        <template v-slot:default>
          <tr v-for="(item, index) in results" :key="index">
            <td>{{ index }}</td>
            <td>{{ fdate(item._source.DATE) }}</td>
            <td>{{ item._source.CATEGORY }}</td>
            <td>{{ item._source.TITLE }}</td>
            <td>
              <v-btn @click="onClickBtn"
                ><v-icon>mdi-dots-horizontal</v-icon></v-btn
              >
            </td>
          </tr>
        </template>
      </v-simple-table>
      <v-dialog v-model="dialog" activator>
        <v-card>
          <v-card-title>test</v-card-title>
          <v-card-actions>
            <v-btn block @click="dialog = false">close</v-btn>
          </v-card-actions>
        </v-card>
      </v-dialog>
    </v-content>
  </v-app>
</template>

<script>
/* eslint-disable no-console */
import axios from "axios";
import dayjs from "dayjs";

export default {
  name: "app",
  data: function() {
    return {
      dialog: false,
      results: "test"
    };
  },
  mounted() {
    axios({
      method: "post",
      url: "http://127.0.0.1:9200/livedoor_news/_search",
      headers: { "Content-Type": "application/json" },
      data: JSON.stringify({
        size: "10",
        sort: { DATE: { order: "asc" } }
      })
    }).then(response => {
      this.results = response.data.hits.hits;
    });
  },
  methods: {
    fdate(text) {
      return dayjs(text).format("YYYY/MM/DD HH:mm");
    },
    onClickBtn() {
      this.dialog = true;
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
