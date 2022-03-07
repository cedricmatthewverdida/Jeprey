<template>
    <div class="d-flex justify-center" style="margin-top:150px">
        <SignIn/>
    </div>
</template>

<script>
    import SignIn from "~/components/form/signin.vue"
    import { mapActions } from 'vuex'
    export default {
        layout: 'newuser',


        methods: {
            ...mapActions(['loggedin']),
        },

        components: {
            SignIn
        },

        mounted() {
            this.loggedin();
        }
        
    }
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Permanent+Marker&display=swap');
.glass_effect{
    backdrop-filter: blur(0px) saturate(180%);
    -webkit-backdrop-filter: blur(0px) saturate(180%);
    background-color: rgba(17, 25, 40, 0.75);
    border-radius: 12px;
    border: 1px solid rgba(255, 255, 255, 0.125);
}


.logo {
    
    font: 10.5em 'Permanent Marker';
}

.logo-copy {
    fill: none;
    stroke: white;
    stroke-dasharray: 6% 29%;
    stroke-width: 5px;
    stroke-dashoffset: 0%;
    animation: stroke-offset 5.5s infinite linear;
}

.logo-copy:nth-child(1){
	stroke: #4D163D;
	animation-delay: -1;
}

.logo-copy:nth-child(2){
	stroke: #840037;
	animation-delay: -2s;
}

.logo-copy:nth-child(3){
	stroke: #BD0034;
	animation-delay: -3s;
}

.logo-copy:nth-child(4){
	stroke: #BD0034;
	animation-delay: -4s;
}

.logo-copy:nth-child(5){
	stroke: #FDB731;
	animation-delay: -5s;
}

@keyframes stroke-offset{
	100% {stroke-dashoffset: -35%;}
}
</style>