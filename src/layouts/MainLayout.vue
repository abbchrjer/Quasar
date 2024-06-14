<template>
   <div class="q-pa-md">
    <q-layout view="hHh Lpr lff" class="shadow-2 rounded-borders">
      <q-header elevated class="bg-black">
<q-toolbar>
          <q-btn flat @click="drawerLeft = !drawerLeft" round dense icon="menu" />
          <q-toolbar-title>Bil</q-toolbar-title>
        </q-toolbar>
      </q-header>
      <!-- <q-drawer
        v-model="drawerLeft"
        show-if-above
        :width="100"
        :breakpoint="50"
        elevated
        class="bg-primary text-white"
      >
        <q-scroll-area class="fit">
          <div class="q-pa-sm">
            Log

          </div>
        </q-scroll-area>
      </q-drawer> -->
    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
  </div>
</template>

<script>
import { defineComponent, ref } from "vue";
export default defineComponent({
  name: "MainLayout",
  setup () {
    return {
      drawerLeft: ref(false),
    }
  }
});
</script>
