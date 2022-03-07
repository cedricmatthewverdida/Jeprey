<template>
  
  <v-form v-model="valid">
    <v-card dark width="600">


      <v-card-text class="pa-15">

        <v-text-field
          dark
          v-model="username"
          :rules="usernameRules"
          :counter="10"
          label="Username"
          required
        ></v-text-field>

        <v-text-field
          dark
          v-model="email"
          :rules="emailRules"
          label="E-mail"
          required
        ></v-text-field>

        <v-text-field
          dark
          v-model="password"
          :rules="passwordRules"
          label="Password"
          required
        ></v-text-field>

        <v-btn
          x-large
          block
          :disabled="!valid"
          @click="createAccount()"
        >
          Signup
        </v-btn>

      </v-card-text>

    </v-card>
  </v-form>
</template>



<script>

  import Moralis from "moralis"

  export default {
    data: () => ({
      
      valid: false,
      
      email: '',
      username: '',
      password: '',


      usernameRules: [
        v => !!v || 'Username  is required',
        v => v.length <= 10 || 'Name must be less than 10 characters',
      ],

      passwordRules: [
        v => !!v || 'Password is required',
        v => /^(?=.*\d)(?=.*[!@#$%^&*])(?=.*[a-z])(?=.*[A-Z]).{8,}$/.test(v) || 'Password must contain atleast one uppercase, lowercase, number and special characters',
      ],

      emailRules: [
        v => !!v || 'E-mail is required',
        v => /.+@.+/.test(v) || 'E-mail must be valid',
      ],


    }),

    methods: {
      async createAccount () {

        const user = new Moralis.User();

        user.set("username", this.username);
        user.set("password", this.password);
        user.set("email", this.email);
        
        try {
          await user.signUp();
          this.$store.dispatch('snackbar/setSnackbar', {
              text :  "Account created successfuly",
              color : 'success'
          });
        } catch (error) {
          this.$store.dispatch('snackbar/setSnackbar', {
              text :  error.message,
              color : 'error'
          });
        }
      }
    }
  }
</script>