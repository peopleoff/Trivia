<template>
  <v-app dark>
    <Navigation/>
    <v-content>
      <router-view/>
    </v-content>
    <Footer/>
  </v-app>
</template>

<script>
import Footer from "@/components/includes/Footer.vue";
import Navigation from "@/components/includes/Navigation.vue";

export default {
  name: "App",
  components: {
    Footer,
    Navigation
  },
  data() {
    return {
      //
    };
  }
};
</script>
