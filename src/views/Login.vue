<template>
  <v-app id="inspire">
    <v-main>
      <v-container class="fill-height" fluid>
        <v-row align="center" justify="center">
          <v-col cols="12" sm="8" md="4">
            <v-card>
              <v-list-item id="cardHeader" size="150">
                <v-list-item-content>
                  <v-list-item-title
                    class="d-flex align-center justify-center pa-3 mx-auto headline"
                  >Sign In</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
              <v-spacer></v-spacer>
              <v-card-text>
                <v-form>
                  <label for="email">Email</label>
                  <v-text-field
                    name="email"
                    label="Email address"
                    v-model="username"
                    prepend-icon="mdi-account"
                    outlined
                    dense
                  ></v-text-field>
                  <label for="password">Password</label>
                  <v-text-field
                    id="password"
                    label="Password"
                    name="password"
                    :append-icon="valuePass ? 'mdi-eye-off' : 'mdi-eye'"
                    @click:append="() => (valuePass = !valuePass)"
                    :type="valuePass ? 'password' : 'text'"
                    prepend-icon="mdi-lock"
                    v-model="password"
                    class="border-design"
                    outlined
                    dense
                  ></v-text-field>
                  <div
                    v-show="isError === true"
                    style="background-color: #ff9999; padding:5px; margin-bottom:4px;"
                  >
                    <span style="color: red">{{errorMsg}}</span>
                  </div>
                </v-form>
              </v-card-text>
              <center>
                <v-btn id="btnLogin" class="mb-5" block outlined @click="login()">Sign in</v-btn>
              </center>
              <div align="center" justify="center">
                <span>
                  Don't have an account?
                  <router-link to="register">Sign up</router-link>
                  <br>
                  <br>
                </span>
              </div>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import axios from "axios";
export default {
  name: "Login",
  data() {
    return {
      username: "",
      password: "",
      valuePass: true,
      isError: false,
      errorMsg: ""
    };
  },
  methods: {
    login() {
      let data = {
        username: this.username,
        password: this.password
      };
      axios
        .post("https://api.baseplate.appetiserdev.tech/api/v1/auth/login", data)
        .then(response => {
          if (response.data.success === true) {
            localStorage.setItem("isAuthenticated", true);
            this.$router.push("success-page");
          }
        })
        .catch(error => {
          let errorObject = JSON.parse(JSON.stringify(error.response));
          console.log(errorObject.data.message);
          this.isError = true;
          this.errorMsg = "Username/Email or Password in incorrect";
          // this.errorMsg = errorObject.data.message;
        });
    }
  }
};
</script>

<style scoped>
#btnLogin,
.border-design {
  border-color: purple !important;
  border-width: 2px !important;
  color: purple !important;
}
.v-btn {
  width: 85% !important;
  min-width: 85% !important;
  margin-left: 5%;
}
#cardHeader {
  background-color: purple;
  color: white !important;
}
</style>
