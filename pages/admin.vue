<template>
    <div>
    <CreatePost
      v-model="openmodal"
      :users="users"
    />
    <v-data-table
        :headers="headers"
        :items="users"
        :items-per-page="8"
        class="elevation-1"
    >
       
        <template v-slot:item.actions="{ item }">

        <v-icon
            small
            @click="accept_pending(item)"
        >
            mdi-checkbox-marked-circle
        </v-icon>



        <v-icon
            small
            @click="delete_user(item)"
        >
            mdi-delete
        </v-icon>

        </template>

    </v-data-table>

    

    <v-dialog
      v-model="dialog"
      persistent
      max-width="290"
    >
      <template v-slot:activator="{ on, attrs }">
        <v-btn
          color="primary"
          dark
          v-bind="attrs"
          v-on="on"
        >
          Add new item
        </v-btn>

        <v-btn
          color="primary"
          dark
          @click="openmodal = true"
        >
          Create post
        </v-btn>
      </template>
      <v-card>
        <v-card-title class="text-h5">
          Add item
        </v-card-title>
        
        <v-card-actions>
          <v-spacer></v-spacer>

          <form>
            <v-text-field
              v-model="username"
              label="username"
            ></v-text-field>

            <v-text-field
              v-model="email"
              label="E-mail"
            ></v-text-field>

            <v-combobox
                v-model="type"
                :items="items"
                outlined
            ></v-combobox>

            <v-text-field
              v-model="password"
              label="password"
              type="password"
            ></v-text-field>

            <div class="d-flex justify-center">
              <v-btn
                class="mr-4"
                 @click="add_item()"
              >
                Login
              </v-btn>

              <v-btn
                class="mr-4"
                 @click="dialog=false"
              >
                cancel
              </v-btn>

              


            </div>
            
          </form>

        

        </v-card-actions>
      </v-card>
    </v-dialog>
    </div>
</template>

<script>
    import Moralis from 'moralis';
    import CreatePost from '~/components/post/createpost.vue';
    export default {

        components:{
          CreatePost
        },
        data() {
            return {
              items:["student","teacher"],
              openmodal: false,
              type:'',
              email:'',
              password:'',
              username:'',
              dialog:false,
              users:[],
              headers: [
              {
                text: 'id',
                align: 'start',
                sortable: false,
                value: 'id',
              },
              { text: 'name', value: 'attributes.username' },
              { text: 'type', value: 'attributes.type' },
              { text: 'email', value: 'attributes.email' },
              { text: 'Actions', value: 'actions', sortable: false }

              
              ],
            }
        }
        ,

        methods:{
            async get_all_workers () {
                this.user_type = Moralis.User.current().get('type');
                const user = await Moralis.Cloud.run("get_all_user");
                this.users = user;
            },
            
            async delete_user(item){
              try {
                //alert(0/0)
                  const appoint = Moralis.Object.extend('User');
                  const del_appoint = new appoint();
                  del_appoint.unset(item)
                  await del_appoint.destroy();
                  alert('removed')

                } catch (error) {
                  alert(error.message)
                  
                }
            },

            async add_item(){
              const user = new Moralis.User();

              user.set("username", this.username);
              user.set("password", this.password);
              user.set("email", this.email);
              user.set("type", this.type);
              
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
              alert("hello")
              this.dialog = false
              location.reload();
            }
        },
        mounted(){
          this.get_all_workers();
        }



        
    }
</script>

<style lang="scss" scoped>

</style>