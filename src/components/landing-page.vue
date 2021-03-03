<template>
  <b-container fluid>
    <!-- navbar section -->
    <section>
      <b-navbar toggleable="lg">
        <b-navbar-brand href="#"><h4>Your Uni</h4></b-navbar-brand>
        <b-navbar-toggle target="nav-collapse"></b-navbar-toggle>
        <b-collapse id="nav-collapse" is-nav>
          <!-- Right aligned nav items -->
          <b-navbar-nav class="ml-auto">
            <b-nav-item class="active">Product </b-nav-item>
            <b-nav-item>Download </b-nav-item>
            <b-nav-item>Pricing </b-nav-item>
          </b-navbar-nav>
        </b-collapse>
      </b-navbar>
    </section>

    <section>
      <b-row align-v="center">
        <b-col> </b-col>
        <b-col class="col-12 col-md-5 text-center">
          <h1>Find the university that’s right for you.</h1>
          <p class="description">
            Tenetur ex explicabo et illo. Recusandae fugit eius voluptatem.
            Voluptas atque autem totam.
          </p>
        </b-col>
        <b-col class="col-12 col-md-5 text-center header-image">
          <b-img src="/images/header_image.png" />
        </b-col>
      </b-row>
    </section>

    <section class="p-40">
      <b-row>
        <b-col class="col-10 col-md-4 mb-3 mt-3 pl-4">
          <b-input placeholder="Search..." v-on:keyup="debounceInput" />
        </b-col>
      </b-row>
      <b-row>
        <b-col
          class="col-12 col-md-3"
          v-for="(item, index) in list"
          :key="index"
        >
          <card :cardData="item" />
        </b-col>
        <b-col class="info-text">
          <h4 v-if="!list || !list.length">Nothing match your search.</h4>
        </b-col>
      </b-row>
    </section>
  </b-container>
</template>

<style scoped>
.navbar {
  margin-bottom: 50px;
}
.navbar a {
  font-size: 17px;
  color: #000000 !important;
}
li.nav-item.active {
  border-bottom: 2px solid #e16259;
}
h1 {
  font-weight: bold;
  font-size: 69px;
  color: #000000;
}
h4 {
  font-weight: bold;
  font-size: 24px;
  margin-left: 25px;
}
.description {
  font-size: 18px;
  color: #000000;
}
.info-text {
  font-family: Inter;
  color: rgba(0, 0, 0, 0.6);
  text-align: center;
}

/* Tablet & Mobile Modes */
@media only screen and (max-width: 600px) {
  h1 {
    font-size: 46px;
  }
  .header-image img {
    width: 100%;
  }
}
</style>

<script>
import card from "./cards";
import _ from "lodash";
export default {
  name: "landing-page",
  components: {
    card,
  },

  data: () => ({
    list: null,
    backuplist: null,
  }),
  created() {
    this.init();
  },
  methods: {
    // right now filter on bases of three property just checking start with,
    // we can add here .include filter as well
    debounceInput: _.debounce(function (e) {
      let searchQuery = e.target.value.toLowerCase();
      if (searchQuery) {
        this.list = this.backuplist.filter((item) => {
          return (
            item.INSTNM.toLowerCase().startsWith(searchQuery) ||
            item.CITY.toLowerCase().startsWith(searchQuery) ||
            item.STABBR.toLowerCase().startsWith(searchQuery)
          );
        });
      } else {
        this.list = this.backuplist;
      }
    }, 500),
    init() {
      this.loadJSON((response) => {
        this.list = this.csvToJSON(response);
        this.backuplist = this.list;
      });
    },

    /* GET request to read csv */
    loadJSON(callback) {
      var xobj = new XMLHttpRequest();
      xobj.overrideMimeType("text/csv");
      xobj.open(
        "GET",
        "https://gist.githubusercontent.com/simonlast/d5a86ba0c82e1b0d9f6e3d2581b95755/raw/f608b9b896dd3339df13dae317998d5f24c00a50/edu-scorecard.csv",
        true
      );
      xobj.onreadystatechange = function () {
        if (xobj.readyState == 4 && xobj.status == "200") {
          callback(xobj.responseText);
        }
      };
      xobj.send(null);
    },

    /* convert csv to json */
    csvToJSON(csv) {
      const lines = csv.split("\n");
      const result = [];
      const headers = lines[0].split(",");

      for (let i = 1; i < lines.length; i++) {
        if (!lines[i]) continue;
        const obj = {};
        const currentline = lines[i].split(",");

        for (let j = 0; j < headers.length; j++) {
          obj[headers[j]] = currentline[j];
        }
        result.push(obj);
      }
      return result;
    },
  },
};
</script>
