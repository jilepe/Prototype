<template>
<ion-content class="bg">
<div class="grid min-h-screen place-items-center">
    <div class="w-11/12 p-12 bg-white sm:w-8/12 md:w-1/2 lg:w-5/12">
     <h1 class="text-xl font-semibold">Hello there 👋, <span class="font-normal">Please fill in your information to register</span></h1>
     <form class="mt-6">
        <div class="flex w-full">
            <div class="grid grid-cols-1 px-3 w-1/2">
                <ion-label class="block mt-2 text-xs font-semibold text-gray-600 uppercase">First Name *</ion-label>
                <ion-input v-model="first" class="py-2 px-3 rounded-lg border-2 border-teal-400 mt-1 focus:outline-none focus:ring-2 focus:ring-purple-600 focus:border-transparent" type="text" placeholder="First Name" />
            </div>
            <div class="grid grid-cols-1 px-3 w-1/2">
                <ion-label class="block mt-2 text-xs font-semibold text-gray-600 uppercase">Middle Name *</ion-label>
                <ion-input v-model="middle" class="py-2 px-3 rounded-lg border-2 border-teal-400 mt-1 focus:outline-none focus:ring-2 focus:ring-purple-600 focus:border-transparent" type="text" placeholder="Middle Name" />
            </div>
        </div>
        <div class="flex w-full">
            <div class="grid grid-cols-1 px-3 w-1/2">
                <ion-label class="block mt-2 text-xs font-semibold text-gray-600 uppercase">Last Name *</ion-label>
                <ion-input v-model="last" class="py-2 px-3 rounded-lg border-2 border-teal-400 mt-1 focus:outline-none focus:ring-2 focus:ring-purple-600 focus:border-transparent" type="text" placeholder="Last Name" />
            </div>
            <div class="grid grid-cols-1 px-3 w-1/2">
                <ion-label class="block mt-2 text-xs font-semibold text-gray-600 uppercase">Suffix</ion-label>
                <ion-input v-model="suffix" class="py-2 px-3 rounded-lg border-2 border-teal-400 mt-1 focus:outline-none focus:ring-2 focus:ring-purple-600 focus:border-transparent" type="text" placeholder="Suffix" />
            </div>
        </div>
        <div class="grid grid-cols-1 px-3 w-1/2">
            <ion-label class="block mt-2 text-xs font-semibold text-gray-600 uppercase">Purok *</ion-label>
            <ion-select :value = "purok" @ionChange = "purok=$event.target.value" class="py-2 px-3 rounded-lg border-2 border-teal-400 mt-1 focus:outline-none focus:ring-2 focus:ring-purple-600 focus:border-transparent" placeholder="Purok">
                <ion-select-option value= "purok1">purok 1</ion-select-option>
                <ion-select-option value= "purok2">purok 2</ion-select-option>
                <ion-select-option value= "purok3">purok 3</ion-select-option>
            </ion-select>
        </div>
        <div class="grid grid-cols-1 px-3 w-3/5">
            <ion-label class="block mt-2 text-xs font-semibold text-gray-600 uppercase">Phone Number *</ion-label>
            <ion-input v-model="phone" class="py-2 px-3 rounded-lg border-2 border-teal-400 mt-1 focus:outline-none focus:ring-2 focus:ring-purple-600 focus:border-transparent" type="text" placeholder="Phone Number" />
        </div>
        
        <div class="grid grid-cols-1 px-3 w-3/5">
            <ion-label class="block mt-2 text-xs font-semibold text-gray-600 uppercase">Password *</ion-label>
            <ion-input v-model="password1" class="py-2 px-3 rounded-lg border-2 border-teal-400 mt-1 focus:outline-none focus:ring-2 focus:ring-purple-600 focus:border-transparent" type="password" placeholder="Password" />
        </div>
        
        <div class="grid grid-cols-1 px-3 w-3/5">
            <ion-label class="block mt-2 text-xs font-semibold text-gray-600 uppercase">Confirm Password *</ion-label>
            <ion-input v-model="password2" class="py-2 px-3 rounded-lg border-2 border-teal-400 mt-1 focus:outline-none focus:ring-2 focus:ring-purple-600 focus:border-transparent" type="password" placeholder="Confirm Password" />
        </div>

         <p @click="register" class="w-full h-10 rounded-full bg-amber-500 hover:bg-amber-300 mt-5 text-center cursor-pointer text-xl text-white font-medium py-2">Sign Up</p>         
         <a href="/login"><p class="flex justify-between inline-block mt-4 text-xs text-gray-500 cursor-pointer hover:text-black">Already registered?</p></a>
        
     </form>
    </div>
</div>

</ion-content>  
</template>

<script lang="ts">
import { 
  IonContent,
  IonLabel,
  IonInput,
  IonSelect,

} from '@ionic/vue';
import { defineComponent, onMounted, ref  } from 'vue';
import axios from 'axios';
import global from '../main';

export default defineComponent({
  name: 'SignUp',
  components: {
    IonContent,
    IonLabel,
    IonInput,
    IonSelect,
  }, 
  data(){
      return{
      first:'',
      middle:'',
      last:'',
      suffix:'',
      purok:'',
      phone:'',
      password1:'',
      password2:'',
      }
  },setup(){   
      const {state} = global;
      return{
          state,
    }
  },
  methods:{
      register(){
          if( this.first == "" ||
            this.middle == "" ||
            this.last == "" ||
            this.phone == "" ||
            this.password1 == "" ){
                alert("Please dont leave entities with '*' empty! Thank you!");
        } else if (this.purok == ""){
            alert("Please select a purok! Thank you!");     
        } else if (this.password2 == ""){
            alert("Please confirm your password! Thank you!");      
        } else if (this.password2 != this.password1){
            alert("Passwords do not match! Please retry, Thank you!");     
        } else {
             axios.post("http://localhost/bcisdb/signup.php", null, {
                params:{
                 "first":this.first,
                 "middle":this.middle,  
                 "last":this.last,
                 "suffix":this.suffix,
                 "purok":this.purok,
                 "phone":this.phone,
                 "password":this.password1,
                } 
             }).
             then( (response) => {
           axios.post("http://localhost/bcisdb/login.php", null, {
                params:{
                 "phone":this.phone,
                 "password":this.password1,
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
           
             }).
             catch(
                 function(error){
                     alert(error);
                 }
             );
        }

      }
  },
});
</script>

<style scoped>
.bg{
  --ion-background-color:#EFEFEF;
}

</style>
