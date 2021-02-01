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
                  >Verification</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
              <v-spacer></v-spacer>
              <br>
              <div v-if="isError !== true" id="form">
                <v-text-field
                  name="verificationCode"
                  label="Verification Code"
                  v-model="verificationCode"
                  outlined
                  dense
                ></v-text-field>
              </div>
              <div v-else id="form">
                <label style="color:red" for="verificationCode">{{errorMsg}}</label>
                <v-text-field
                  name="verificationCode"
                  label="Verification Code"
                  v-model="verificationCode"
                  error
                  outlined
                  dense
                ></v-text-field>
              </div>
              <center>
                <v-btn id="btnLogin" class="mb-5" block outlined @click="verify()">Submit</v-btn>
                <br>
              </center>
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
  name: "Verification",
  data() {
    return {
      verificationCode: "",
      errorMsg: "",
      isError: false
    };
  },
  beforeCreate() {
    let config = {};
    config.headers = {
      Authorization: "Bearer " + localStorage.getItem("access_token"),
      Accept: "application/json"
    };
    this.config = config;
  },
  methods: {
    verify() {
      let data = {
        token: this.verificationCode,
        via: "email"
      };
      axios
        .post(
          "https://api.baseplate.appetiserdev.tech/api/v1/auth/verification/verify",
          data,
          this.config
        )
        .then(response => {
          if (response.data.success === true) {
            this.$router.push("login");
            localStorage.clear();
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
  width: 25% !important;
  min-width: 25% !important;
}
#cardHeader {
  background-color: purple;
  color: white !important;
}
#form {
  margin-left: 10%;
  margin-right: 10%;
}
</style>