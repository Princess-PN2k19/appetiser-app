<template>
  <v-form>
    <v-app id="inspire">
      <v-main>
        <v-container class="fill-height" fluid>
          <v-row align="center" justify="center">
            <v-col cols="12" sm="8" md="4">
              <v-card class="elevation-12">
                <v-list-item id="cardHeader" size="150">
                  <v-list-item-content>
                    <v-list-item-title
                      class="d-flex align-center justify-center pa-3 mx-auto headline"
                    >Sign Up</v-list-item-title>
                  </v-list-item-content>
                </v-list-item>
                <v-card-text>
                  <v-form autocomplete="off">
                    <label for="email">
                      Email
                      <span style="color:red">*</span>
                    </label>
                    <v-text-field
                      ref="email"
                      label="Email"
                      name="email"
                      prepend-icon="mdi-email"
                      type="text"
                      v-model="email"
                      class="border-design"
                      outlined
                      dense
                    ></v-text-field>
                    <label for="full_name">
                      Fullname
                      <span style="color:red">*</span>
                    </label>
                    <v-text-field
                      id="full_name"
                      label="Fullname"
                      name="full_name"
                      prepend-icon="mdi-account"
                      type="text"
                      v-model="full_name"
                      class="border-design"
                      outlined
                      dense
                    ></v-text-field>
                    <label for="password">
                      Password
                      <span style="color:red">*</span>
                    </label>
                    <v-text-field
                      autocomplete="current-password"
                      name="password"
                      prepend-icon="mdi-lock"
                      label="Enter password"
                      v-model="password"
                      :append-icon="valuePass ? 'mdi-eye-off' : 'mdi-eye'"
                      @click:append="() => (valuePass = !valuePass)"
                      :type="valuePass ? 'password' : 'text'"
                      outlined
                      dense
                    ></v-text-field>
                    <label for="password_confirmation">
                      Confirm Password
                      <span style="color:red">*</span>
                    </label>
                    <v-text-field
                      id="password"
                      label="Confirm Password"
                      :append-icon="valueConfirmPass ? 'mdi-eye-off' : 'mdi-eye'"
                      @click:append="() => (valueConfirmPass = !valueConfirmPass)"
                      :type="valueConfirmPass ? 'password' : 'text'"
                      name="password_confirmation"
                      prepend-icon="mdi-lock"
                      v-model="password_confirmation"
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
                  <v-btn id="btnLogin" class="mb-5" block outlined @click="signUp">Sign Up</v-btn>
                </center>
                <div align="center" justify="center">
                  <span>
                    Already have an account?
                    <router-link to="login">Sign in</router-link>
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
  </v-form>
</template>

<script>
import axios from "axios";
export default {
  name: "Registration",
  data() {
    return {
      email: "",
      full_name: "",
      password: "",
      password_confirmation: "",
      valuePass: true,
      valueConfirmPass: true,
      errorMsg: "",
      isError: false
    };
  },
  methods: {
    signUp() {
      let data = {
        email: this.email,
        full_name: this.full_name,
        password: this.password,
        password_confirmation: this.password_confirmation,
        errorMsg: "",
        isError: false
      };
      axios
        .post(
          "https://api.baseplate.appetiserdev.tech/api/v1/auth/register",
          data
        )
        .then(response => {
          if (response.data.success == true) {
            localStorage.setItem(
              "access_token",
              response.data.data.access_token
            );
            this.isError = true;
            this.$router.push("verify");
          }
        })
        .catch(error => {
          let errorObject = JSON.parse(JSON.stringify(error.response));
          this.isError = true;
          this.errorMsg = errorObject.data.message;
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
  margin-left: 6%;
}
#cardHeader {
  background-color: purple;
  color: white !important;
}
</style>