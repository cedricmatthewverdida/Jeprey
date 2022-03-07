<template>
    <div>
        <div class="d-flex justify-center">

            <SignUpForm/>

        </div>
    </div>
</template>

<script>
    import SignUpForm from "~/components/form/signup.vue"
    import Snack from '~/components/snackbar.vue'
    export default {

        layout: 'newuser',

        components:{
            Snack,
            SignUpForm
        },


        
    }
</script>

<style lang="scss" scoped>

</style>