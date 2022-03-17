<template>
    <v-dialog
      v-model="show"
      width="700"
    >

      <v-card>
        <v-card-title>
          Create Post
          <v-spacer></v-spacer>
          <v-btn
            color="primary"
            icon
            large
            @click.stop="show=false"
          >
            <v-icon>
                mdi-close-circle-outline
            </v-icon>
          </v-btn>
        </v-card-title>

        <v-card-subtitle>
        <a href="https://www.markdownguide.org/cheat-sheet/" target="_blank">Markdown</a> syntax is supported.
        </v-card-subtitle>

        <v-card-text>


          
          <v-combobox
            v-model="usertosee"
            filled
            rounded
            :items="users"
            :item-text="(users) => users.get('username')"
            :item-value="(users) => users.id"
            label="Users who can view the post"
            multiple
          >
            <template slot='item' slot-scope='{ item }'>
                {{item.get('username')}}
            </template>
          </v-combobox>
      

          <v-textarea
            v-model="description"
            name="input-7-1"
            label="What's on your mind ?"
            hide-details
            filled
            rounded
            :disabled="loads == true"
          ></v-textarea>

        </v-card-text>

        <v-divider></v-divider>

        <v-card-actions>
          <v-btn
            :loading="loads == true"
            color="primary"
            :disabled="description == '' || loads == true"
            block
            rounded
            x-large
            @click="post()"
          >
          Post
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
</template>



<script>
    import Moralis from 'moralis'
    export default {
        data () {
            return {
                loads: false,
                dialog: false,
                description: '',
                usertosee: []
            }
        },

        watch:{
          file : function(v){
            console.log(v)
          }
        },

        props: {
          value: Boolean,
          users: Array
        },

        computed: {
          show: {
            get () {
              return this.value
            },
            set (value) {
              this.$emit('input', value)
            }
          }
        },

        methods: {
            async post () {

                
                if (Moralis.User.current()) {
                  this.loads = true
                  const CreatePost =  Moralis.Object.extend("Post");
                  const Post = new CreatePost();

                  await Post.save({
                    postedby: Moralis.User.current(),
                    description: this.description,
                    students: this.usertosee
                  })
                  .then((save) => {
                   
                    this.$store.dispatch('snackbar/setSnackbar', {
                        text : 'Created new post #' + save.id,
                        color : 'success',
                        icon: 'mdi-check'
                    });
                    
                  }, (error) => {
                    this.$store.dispatch('snackbar/setSnackbar', {
                        text : error.message,
                        color : 'error'
                    });
                  });
                  
                  
                  this.loads = false
                } else {
                  this.$store.dispatch('snackbar/setSnackbar', {
                        text : "You need to login",
                        color : 'error'
                  });
                }
            }
        }

    }
</script>


<style>

.limitwidth{
  max-width: 200px;
}

</style>