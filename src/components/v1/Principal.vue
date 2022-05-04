<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-12">
        <NavbarVue></NavbarVue>
      </div>
    </div>
    <div class="row justify-content-md-center">
      <div class="col-12 text-center">
        <CardGW>
          <Crypt></Crypt>
        </CardGW>
      </div>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import NavbarVue from "./NavbarVue.vue";
import CardGW from "./CardGW.vue";
//import Crypt from "./Crypt.vue";
import { defineAsyncComponent } from "vue";

export default {
  name: "Principal",
  components: {
    NavbarVue,
    CardGW,
    Crypt: defineAsyncComponent(() =>
      import("./Crypt.vue")
    ),
  },
};
</script>

<style>
</style>