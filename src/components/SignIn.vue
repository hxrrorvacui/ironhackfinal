<template>

  <div class="container testingSingIn">
    <div class="header">
      <div class="header-description">
        <h3 class="header-title">Welcome to OrganizeMe</h3>
        <br>
          <p class="header-subtitle">Start organizing your tasks!</p>
        </div> <!--end of header description div-->
    </div> <!--end of header div-->
  </div> <!--end of container testingSignIn-->

  <div class="box"> 
    <form @submit.prevent="signIn" class="form-sign-in">
      <div class="form">
        <div class="form-input">
          <label class="input-field-label">E-mail</label>
          <input
            type="email"
            class="input-field"
            placeholder="example@gmail.com"
            id="email"
            v-model="email"
            required
          />
        </div>
        <div class="form-input">
          <label class="input-field-label">Password</label>
          <input
            type="password"
            class="input-field"
            placeholder="**********"
            id="password"
            v-model="password"
            required
          />
        </div>
        <br>
        <button class="button" type="submit">Sign In</button>
        <p>
          Have an account?
          <PersonalRouter
            :route="route"
            :buttonText="buttonText"
            class="sign-up-link"
          />
        </p>
      </div>
  </form>

  </div>
  <div v-show="errorMsg">{{errorMsg}}</div>
  <!--end of container div-->




</template>

<script setup>
import { ref, computed } from "vue";
import PersonalRouter from "./PersonalRouter.vue";
import { supabase } from "../supabase";
import { useRouter } from "vue-router";
import { useUserStore } from "../stores/user";
import { storeToRefs } from "pinia";

// Route Variables
const route = "/auth/signup";
const buttonText = "Sign Up";

// Input Fields
const email = ref("");
const password = ref("");


// Error Message
const errorMsg = ref("");

// Router to push user once SignedUp to Log In
const redirect = useRouter();

// Arrow function to Signin user to supaBase
const signIn = async () => {
try {
await useUserStore().signIn(email.value, password.value);
redirect.push({ path: "/" });
} catch (error) {
errorMsg.value = error.message;
setTimeout(() => {
errorMsg.value = null;
}, 5000);
}
};
</script>

<style></style>