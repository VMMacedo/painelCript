<template>
  <div>
    <h5 class="card-title text-center">
      <img :src="Crypto.logo" class="img-fluid" width="60" alt="" />
    </h5>
    <h5 class="text-center">{{ Crypto.coin }}</h5>
    <p class="card-text">
      Cotação: <b>{{ cryptPrice }}</b>
    </p>
  </div>
</template>

<script>
/* eslint-disable */
export default {
  props: {
    Crypto: {
      type: Object,
      required: true,
      default() {
        // default function receives the raw props object as argument
        return { message: "Cerregando..." };
      },
    },
  },
  computed: {
    cryptPrice() {
      if (this.Crypto.prices.BRL) {
        return parseFloat(this.Crypto.prices.BRL).toLocaleString("pt-br", {
          style: "currency",
          currency: "BRL",
        });
      }
    },
  },
};
</script>

<style>
</style>