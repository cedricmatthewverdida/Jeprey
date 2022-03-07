<template>
    <div>
        <div class="d-flex justify-center">

            <Profilepic class="mt-15"/>

        </div>
    </div>
</template>

<script>
    import Profilepic from '~/components/profile/profilepic.vue'
    export default {

        layout: 'newuser',

        components:{
            Profilepic
        },





        data() {
            return {
                yawa:'yawa'
            }
        },



        
    }
</script>

<style lang="scss" scoped>

</style>