<template>
  <v-app id="inspire">
    <navbar></navbar>
    <v-content>
      <v-container class="fill-height" fluid>
        <v-row align="center" justify="center">
          <v-col cols="12" sm="8" md="4">
            <v-card class="elevation-12">
              <v-toolbar color="primary" dark flat>
                <v-toolbar-title>Login form</v-toolbar-title>
                <v-spacer></v-spacer>
                <v-tooltip bottom>
                  <span>Source</span>
                </v-tooltip>
                <v-tooltip right></v-tooltip>
              </v-toolbar>
              <v-form @submit.prevent="loginNow">
                <v-card-text>
                  <v-text-field
                    label="Email"
                    name="login"
                    prepend-icon="person"
                    type="text"
                    v-model="email"
                  ></v-text-field>

                  <v-text-field
                    id="password"
                    label="Password"
                    name="password"
                    prepend-icon="lock"
                    type="password"
                    v-model="password"
                  ></v-text-field>
                </v-card-text>
                <v-card-actions>
                  <v-spacer></v-spacer>
                  <v-btn type="submit" color="primary">Login</v-btn>
                </v-card-actions>
              </v-form>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
import navbar from "../components/navbar";
import axios from "axios";
import Swal from "sweetalert2";
export default {
  components: {
    navbar
  },
  data() {
    return {
      password: "",
      email: ""
    };
  },
  methods: {
    loginNow() {
      let loader = this.$loading.show({
        container: this.fullPage ? null : this.$refs.formContainer,
        canCancel: false,
        loader: "dots",
        color: "#23374d"
      });
      axios({
        method: "POST",
        url: "http://35.225.201.56/user/signin",
        data: {
          email: this.email,
          password: this.password
        }
      })
        .then(({ data }) => {
          loader.hide();
          this.$toast.success("Logged In..", "Success");
          localStorage.setItem("access_token", data.token);
          this.$router.push("/products");
        })
        .catch(error => {
          loader.hide()
          let message =
            (error.response.data && error.response.data.message) ||
            "Failed to login";
          this.$toast.error(message, "Error");
        });
    }
  }
};
</script>

<style>
</style>
