<template>
  <v-app>

    <v-layout v-resize="onResize">
    
    <NavigationBar :width_demension="windowSize.x"/>
    <BottomNavigationBar v-if="windowSize.x <= 1024"/>

    <v-main>
        <Nuxt />
    </v-main>

    
    </v-layout>

  <TheSnackbar />
  
  </v-app>
</template>

<script>
import NavigationBar from '~/components/navigation.vue'
import BottomNavigationBar from '~/components/bottomnavigation.vue'
import { mapActions,mapState } from 'vuex'
import TheSnackbar from '@/components/TheSnackbar.vue';
export default {

  components:{
    NavigationBar,
    BottomNavigationBar,
    TheSnackbar
  },

  data () {
      return {
        windowSize: {
          x: 0,
          y: 0,
        },
      }
  },
  computed:{
    ...mapState(
        [
          'user'
        ]
      ),
  },


  mounted(){
      this.onResize()
      this.loggedin();
      if(this.user.length != 0){
        if(this.user.get('profilepic') == null || this.user.get('profilepic') == undefined ){
          this.$router.push('/addprofile');
        }
      }else{
        this.$router.push('/login');
      }
  },

  methods:{

      onResize () {
        this.windowSize = { x: window.innerWidth, y: window.innerHeight }
      },

      ...mapActions(['loggedin']),
  }

  
}
</script>

