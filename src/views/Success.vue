<template>
  <div>
    <div>
      <template>
        <v-toolbar dense>
          <v-toolbar-title>Appetiser Apps</v-toolbar-title>
          <v-spacer></v-spacer>
          <v-btn @click="logout()" text>
            <span>Logout</span>
          </v-btn>
        </v-toolbar>
      </template>
    </div>
    <div class="body">
      <center>
        <h1>
          <b>Login Successful!</b>
        </h1>
      </center>
    </div>
  </div>
</template>
<script>
export default {
  name: "Success",
  data() {
    return {};
  },
  methods: {
    logout() {
      localStorage.clear();
      this.$router.push("login");
    }
  }
};
</script>
<style scoped>
.body {
  margin-top: 15%;
}
span {
  text-transform: lowercase;
}
span:first-letter {
  text-transform: capitalize;
}
</style>