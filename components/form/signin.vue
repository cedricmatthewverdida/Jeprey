<template>
  
  <v-form v-model="valid">
    <v-card dark width="600">


      <v-card-text class="pa-15">

        <v-text-field
          dark
          v-model="username"
          :rules="usernameRules"
          label="Username"
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
          @click="login()"
        >
          Login
        </v-btn>

         <v-btn @click="$router.push('/register')">
            Register
        </v-btn>

      </v-card-text>

    </v-card>
  </v-form>
</template>



<script>
    import { mapState,mapActions } from 'vuex'
    import Moralis from "moralis"

  export default {
    data: () => ({
      
      valid: false,
      
      username: '',
      password: '',

      usernameRules: [
        v => !!v || 'username  is required',
      ],

      passwordRules: [
        v => !!v || 'Password is required',
      ],


    }),

    computed:{
        ...mapState(['user'])
    },

    methods: {

    ...mapActions(['loggedin']),

      async login () {

        const user = await Moralis.User.logIn(this.username, this.password);

        try {

          await user;
          this.$store.dispatch('snackbar/setSnackbar', {
              text :  "Successfuly loggedin",
              color : 'success'
          });

          if(user){

            this.$store.commit('authorize_loggin', user)
            this.$router.push('/');
            
          }

        } catch (error) {
          this.$store.dispatch('snackbar/setSnackbar', {
              text :  error.message,
              color : 'error'
          });
        }
      }
    },

    
  }
</script>