<script setup>
import { ref, watch } from 'vue';
import { Icon } from '@iconify/vue';

let internetStatus = ref("");
let internetStatusColor = ref("");

const checkInternetStatus = () => {
 watch(() => navigator.onLine, () => {
  const status = navigator.onLine ? "Online" : "Offline";
  const color = navigator.onLine ? "#41b080" : "#f95e5e";
  internetStatus.value = status;
  internetStatusColor.value = color;
 }, {
  deep: true,
  immediate: true,
 });

 requestAnimationFrame(checkInternetStatus)
}

requestAnimationFrame(checkInternetStatus);

</script>

<template>
 <header>
  <nav class="container">
   <div class="branding">
    <img src="../assets/Vue_Logo_Black.png" alt="">
    <h1>Vue Todos</h1>
   </div>

   <div class="internet-status">
    <Icon icon="ph:wifi-high" class="icon" :color="internetStatusColor" width="22" />
    <span :internetStatus="internetStatus" :style="{ color: internetStatusColor }">{{ internetStatus }}</span>
   </div>

   <div>
    <ul class="nav-routes">
     <RouterLink to="/">Home</RouterLink>
     <RouterLink to="/about">About</RouterLink>
    </ul>
   </div>
  </nav>
 </header>
</template>

<style lang="scss" scoped>
header {
 background-color: #f1f1f1;

 nav {
  display: flex;
  align-items: center;
  padding: 25px 16px;
  justify-content: space-between;

  .branding {
   display: flex;
   align-items: center;
   gap: 8px;

   img {
    max-width: 50px;
   }

   h1 {
    font-size: 24px;
   }
  }

  .internet-status {
   display: flex;
   align-items: center;
   padding: 6px;
   border: 0.5px solid #9d9d9d;
   color: #9d9d9d;
   border-radius: 10px;

   span {
    font-size: 14px;
   }
  }

  .nav-routes {
   display: flex;
   flex: 1;
   justify-content: flex-end;
   gap: 12px;
   list-style: none;

   a {
    text-decoration: none;
    color: inherit;
   }
  }
 }
}
</style>