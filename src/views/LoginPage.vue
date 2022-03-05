<template>
<ion-content class="bg">
<div class="content mx-auto">
  <div class="flex justify-center px-6 my-12">
    <div class="w-full xl:w-3/4 lg:w-11/12 flex">
    <div class="bg-orange-300 hidden lg:block lg:w-1/2 bg-cover rounded-l-lg bg-auto bg-no-repeat bg-center"
		style="background-image: url(./assets/icon/image.png)">
    </div>

          <div class="w-full lg:w-1/2 bg-white p-5 rounded-lg lg:rounded-l-none  ">
          <img class="ml-32 scale-75 " src="../assets/images/tinago.png">
          <h3 class="pt-4 text-2xl text-center">Welcome Back!</h3>  
          <form class="px-8 pt-6 pb-8 mb-4 bg-white rounded">
            <div class="mb-4">
              <ion-label class="block mb-2 text-sm font-bold text-gray-700">Phone Number</ion-label>
              <ion-input v-model="phone" class="w-full px-3 py-2 text-sm leading-tight text-gray-700 border rounded shadow appearance-none focus:outline-none focus:shadow-outline" 
              type="text" placeholder="Phone Number">
              <ion-icon :src = "calculator"></ion-icon>
              </ion-input>
              
            </div>

            <div class="mb-4">
              <ion-label class="block mb-2 text-sm font-bold text-gray-700">Password</ion-label>
              <ion-input v-model="password" class="w-full px-3 py-2 text-sm leading-tight text-gray-700 border rounded shadow appearance-none focus:outline-none focus:shadow-outline"
              type="password" placeholder="Password">
              <ion-icon :src = "lockClosed"></ion-icon>
              </ion-input>
            </div>

            <div class="mb-6 text-center">
              <ion-button class="" expand="block" @click="login" >Sign In</ion-button>
            </div>
            <hr class="mb-6 border-t" />
            <div class="text-center">
              <a class="inline-block text-sm text-blue-500 align-baseline hover:text-blue-800"
						href="/signup">	SignUp	</a>
            </div>
            <div class="text-center">
              <a class="inline-block text-sm text-blue-500 align-baseline hover:text-blue-800"
									@click="CheckGlobal" >	Forgot Password?	</a>
            </div>

          </form> 
          </div>
    </div>  
  </div>
</div>

</ion-content>
</template>

<script lang="ts">

import { defineComponent, onMounted, ref  } from 'vue';
import axios from 'axios';
import global from '../main';

import { 
  IonContent, 
  IonButton, 
  IonInput,
  IonIcon, 
} from '@ionic/vue';
import { calculator, lockClosed} from 'ionicons/icons';

export default defineComponent({
  name: 'LoginPage',
  components: {
    IonContent, 
    IonButton,
    IonInput
  },data(){
      return{
          phone: '',
          password: '',
      }
  }, setup(){   
      const {state} = global;
      return{
          calculator,
          lockClosed,
          state,
    }
  },
  methods:{
      CheckGlobal(){
          alert(this.state.residentID);
      },
      login(){
          if(this.phone == ""){
          alert("Enter Phone Number");
          } else if(this.password ==""){
          alert("Enter Password");
          } else {
           axios.post("http://localhost/bcisdb/login.php", null, {
                params:{
                 "phone":this.phone,
                 "password":this.password,
                }}).
           then( (response)=> {
               if(response.data.message == "success"){
                    //alert("Login Successfully");
                    this.state.residentID = response.data.id;
                    this.state.residentPhone = response.data.phone;
                    this.state.residentFirst = response.data.first;
                    this.state.residentLast = response.data.last;
                    this.state.residentSuffix = response.data.suffix;
                    this.state.residentPurok = response.data.purok;
                    this.$router.push("/residentpage");
               } else {
                    alert("Login Failure");
               }
           }).
           catch(function(error){
               alert(error);
           });
          }
      }
  }
});
</script>

<style scoped>
.bg{
  --ion-background-color:#EFEFEF;
}

</style>
