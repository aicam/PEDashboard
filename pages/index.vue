<template>
  <v-row justify="center" align="center">
    <v-col cols="12" sm="8" md="6">
      <v-card>
        <v-card-title class="headline" v-if="isLoggedOut">
          Welcome to AI investment dashboard
        </v-card-title>
        <v-card-title class=" " style="text-align: center" v-else>
          Redirecting ...
        </v-card-title>
        <v-card-text v-if="isLoggedOut">
          AI investment is a cutting-edge service powered by artificial
          intelligence (AI) that revolutionizes the way investors make informed
          decisions. It is designed to support investors by providing a
          comprehensive suite of AI tools, including due diligence analysis and
          deal sourcing capabilities. <br />
          One of the key features of the service is its due diligence analysis
          tool. This tool utilizes AI algorithms to conduct in-depth research on
          potential investment opportunities. It scours vast amounts of
          structured and unstructured data, including financial reports, news
          articles, social media sentiments, and market trends. By analyzing
          this information, the service can quickly identify the strengths,
          weaknesses, risks, and growth potential of a particular investment
          opportunity.
        </v-card-text>

        <v-card-actions>
          <v-btn v-if="isLoggedOut" to="login">
            Log-in to AI-Based Due Diligence Service
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-col>
  </v-row>
</template>

<script>
import api from "@/components/API";
export default {
  name: "IndexPage",
  mounted() {
    this.isLoggedOut = !api.getAuth();
    if (!this.isLoggedOut) window.location.href = "/dashboard";
  },
  data() {
    return {
      isLoggedOut: false,
    };
  },
};
</script>
