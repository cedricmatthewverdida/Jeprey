<template>
    <div>

    </div>
</template>


<script>
    import Moralis from 'moralis'
    export default {

        data() {
            return {
                user: [],
                items: [],
                post: []
            }
        },

        methods: {
            async get_profile () {
                const params =  { username: this.$route.params.id};
                const user = await Moralis.Cloud.run("get_user",params);
                
                if(user.length != 0) {
                    const userid =  { id: user.id };
                    const items = await Moralis.Cloud.run("get_user_item",userid);
                    console.log(user.id);
                }
            },
        },
        mounted(){
            this.get_profile()
        }
    }
</script>
