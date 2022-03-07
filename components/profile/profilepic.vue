<template>
    <v-card
    color="transparent"
    outlined
    width="400"
    dark
    >
        <v-card-title>
            Upload Profile Pic
        </v-card-title>
        <v-card-subtitle>
            image (PNG, JPG, or GIF)
        </v-card-subtitle>
        <v-card-text>

            <div class="image-upload-wrap ml-6">
                <div class="drag-text" v-if="image == null">
                     <h3 style="margin-top:70px;">
                         <v-icon
                         x-large
                         dark
                         >
                             mdi-image
                         </v-icon>
                     </h3>
                </div>
                <div class="d-flex justify-center pa-1" v-else>
                    <v-img
                    height="290px" 
                    width="290px"
                    style="
                    border-radius:10px;
                    margin:auto;
                    "
                    v-if="url != null"
                    :src="url"/>
                </div>
            </div>

            <v-file-input
            dark
            accept="image/*"
            label="Upload profile pic here"
            hint="provide an image (PNG, JPG, or GIF)"
            show-size
            @change="Preview_image"
            v-model="image"
            filled
            rounded
            prepend-icon="mdi-image-plus"
            class="mt-2 mb-3"
            hide-details
            ></v-file-input>

            <v-btn
                color="primary"
                block
                x-large
                :disabled="!upload"
                @click="UploadImage()"
            >
                Upload
            </v-btn>
           
        </v-card-text>
    </v-card>
</template>

<script>
    
    import Moralis from 'moralis'

    export default {


        data() {
            return {
            url: null,
            image: null,
            upload: false
            }
        },

        methods: {

        Preview_image() {
            return this.url = this.image != null ?  URL.createObjectURL(this.image) : null
        },



        async UploadImage () {

            const profile = new Moralis.File(this.image.name, this.image);

            const user = new Moralis.User.current();

            user.set("profilepic", profile)

            
            this.upload = false
            try {
            
            await user.save();

            

            this.$store.dispatch('snackbar/setSnackbar', {
                text :  "Image uploaded success",
                color : 'success'
            });

            setTimeout(() => {
                this.$router.push('/')
            }, 2000);

            } catch (error) {
            this.$store.dispatch('snackbar/setSnackbar', {
                text :  error.message,
                color : 'error'
            });
            this.upload = true
            }
        }


        },

        watch:{
            image : function (v){

                if(v == null){
                    this.upload = false;
                }

                if(v != null){
                    this.upload = true;
                }
            }
        }

    }
</script>

<style lang="scss" scoped>
.image-upload-wrap {
  height: 310px;
  width: 310px;
  border: 4px dashed gray;
  position: relative;
  border-radius:10px;
  
}


.drag-text {
  text-align: center;
}

.drag-text h3 {
  font-weight: 100;
  text-transform: uppercase;
  color: #15824B;
  padding: 60px 0;
}

</style>