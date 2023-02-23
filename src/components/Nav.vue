<template>
<nav>
    <ul class="menu" :class="{ 'show-menu': showMenu }">
      <li><router-link class="nav-link" to="/">Home</router-link></li>
      <li>
        <router-link class="nav-link" to="/account">Your Account</router-link>
      </li>
      <li class="log-out-welcome  burger-hide" v-if="userEmail">
        <p>Welcome, {{ userEmail.split("@")[0] }}</p>
      </li>
      <li v-if="userEmail">
        <button @click="signOut" class="button">Log out</button>
      </li>
    </ul>
    <button class="burger button-burger" @click="showMenu = !showMenu">☰</button>
  </nav>
</template>

<script setup>
// import PersonalRouter from "./PersonalRouter.vue";
import { useUserStore } from "../stores/user";
import { computed, onMounted } from "vue";
import { useRouter } from "vue-router";
import { ref } from "vue";

//constant to save a variable that will hold the use router method
const route = "/";
const buttonText = "Todo app";
const userName = ref("");
// constant to save a variable that will get the user from store with a computed function imported from vue
const getUser = computed(() => useUserStore().user);

// constant that calls user email from the useUSerStore
const userEmail = ref("");
async function getEmail() {
  await useUserStore().fetchUser();
  userEmail.value = useUserStore().user.email;
}
getEmail();
// getUser.email;

// async function that calls the signOut method from the useUserStore and pushes the user back to the Auth view.
const redirect = useRouter();

const signOut = async () => {
  console.log("Entered the sign out function");
  try {
    // call the user store and send the users info to backend to signOut
    await useUserStore().signOut();
    // then redirect user to the homeView
    redirect.push({ path: "/auth/login" });
  } catch (error) {
    console.error(error);
    throw error;
  }
};
// show burger menu
const showMenu = ref(false);
</script>

<style></style>