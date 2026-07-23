<template>
  <v-app>
    <v-main>
      <NavHeader />
      <router-view />
    </v-main>
  </v-app>
</template>

<script setup>
import NavHeader from '@/components/navHeader.vue';


</script>
