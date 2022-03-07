<template>
    <div>

    

    <v-app-bar
        clipped-left
        fixed
        app
        flat
        style="
        border-bottom-left-radius: 10px;
        border-bottom-right-radius: 10px;
        border: 1px solid rgba(209, 213, 219, 0.3)
        
        "
        v-if="width_demension >= 1024"
    >

        <v-toolbar-title>{{title}}</v-toolbar-title>
        <v-spacer></v-spacer>

            <v-tabs
            color="primary"
            icons-and-text
            v-model="activeTab"
            >
                <v-tab v-for="tab in routes" :key="tab.id" :to="tab.link" style="text-transform:none;">
                     
                    {{ tab.title }}

                    <v-icon small class="mr-1">{{tab.icon}}</v-icon>
                </v-tab>

                <v-tabs-items v-model="activeTab" @change="updateRouter($event)">
                    <v-tab-item v-for="tab in routes" :key="tab.id" :to="tab.link">
                        <router-view />          
                    </v-tab-item>
                </v-tabs-items>
            </v-tabs>

        <v-btn icon class="mx-1" v-if="user.length != 0" to="/message">
            <v-icon>mdi-message-processing-outline</v-icon>
        </v-btn>

        <v-menu offset-y>
        <template v-slot:activator="{ on, attrs }">
            <v-btn
            v-if="user.length != 0"
            text
            rounded
            style="
            text-transform:none;
            "
            v-bind="attrs"
            v-on="on"
            >
                <!-- <v-avatar
                size="30"
                class="mr-2"
                v-if="user.length != 0"
                >
                    <v-img
                    :src="user.attributes.profilepic._url"
                    >
                    </v-img>
                </v-avatar> -->
                {{user.get("username")}}
            </v-btn>
        </template>
            <v-list rounded>
                <v-list-item to="/profile">
                <v-list-item-content>
                    <v-list-item-title>Profile</v-list-item-title>
                </v-list-item-content>
                </v-list-item>

                <v-list-item @click="logout_metamask()">
                <v-list-item-content>
                    <v-list-item-title>Logout</v-list-item-title>
                </v-list-item-content>
                </v-list-item>
            </v-list>
        </v-menu>


    

        <v-btn
        depressed
        icon
        to="/login"
        v-if="user.length == 0"
        >
        <v-icon class="mr-2">mdi-login</v-icon>
        </v-btn>
        

    </v-app-bar>

    <v-app-bar
        v-else
        clipped-left
        fixed
        app
        flat
        style="
        border-bottom-left-radius: 10px;
        border-bottom-right-radius: 10px;
        border: 1px solid rgba(209, 213, 219, 0.3)
        "
    >        

    <v-app-bar-nav-icon
    v-if="user.length != 0" 
    @click.stop="drawer = !drawer"
    ></v-app-bar-nav-icon>

    <v-spacer>
        <v-text-field
            prepend-inner-icon="mdi-magnify"
            label="Search"
            clearable
            rounded
            class="mt-7"
        ></v-text-field>
    </v-spacer>

        <v-icon class="mx-1">
            mdi-bell-outline
        </v-icon>


        <v-btn 
        icon
        v-if="user.length != 0"
        to="/message">

        
        <v-icon class="mx-1">
            mdi-message-processing-outline
        </v-icon>

        </v-btn>

        

    </v-app-bar>


    </div>
</template>

<script>
    import { mapState,mapMutations } from 'vuex'
    import Moralis from 'moralis'
    
    export default {
        props: {
            width_demension : 0
        },
        computed:{
            ...mapState(['user']),

            
            isNotIndex : function (){
                return $nuxt.$route.name == 'index' || $nuxt.$route.name == 'message' || $nuxt.$route.name == 'market' ? true : false
            }
        },

        data () {
            return {
            swap_dialog: false,
            balance: undefined,
            activeTab: '',
            routes: [
            {
                id: 1,
                title: 'Home',
                icon: 'mdi-home',
                link: '/'
            },
            
            ],
            drawer:true,
            items: [
                {
                icon: 'mdi-account',
                title: 'Account',
                to: '/account'
                },
                {
                icon: 'mdi-logout',
                title: 'Logout',
                to: '/logout'
                }
            ],
            miniVariant: true,
            right: false,
            rightDrawer: false,
            title: 'Jeprey'
            }
        },
        filters:{
            truncate_address : function (string){
                var startDigit = string.substring(0,7);
                var lastDigit = string.substr(string.length-5);
                return startDigit+"..."+lastDigit
            },
            truncate_balance : function (string){
                let balance = string.toString();
                var startDigit = balance.substring(0,8);
                return startDigit
            }
        },
        methods: {

            updateRouter(val){
                this.$router.push(val)
            },

            ...mapMutations(['authorize_loggin']),

            async logout_metamask() {
                await Moralis.User.logOut();
                this.authorize_loggin([]);
                this.$router.push('/')
            }
        },
        
    }
</script>

