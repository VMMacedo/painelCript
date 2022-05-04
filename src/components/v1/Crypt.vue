<template>
  <div>
    <div>
      <div class="row">
        <div class="col-3">
          <div class="card mb-3" style="max-width: 18rem">
            <div class="card-body">
              <CardBody :Crypto="Bitcoin"></CardBody>
            </div>
          </div>
        </div>
        <div class="col-3">
          <div class="card mb-3" style="max-width: 18rem">
            <div class="card-body">
              <CardBody :Crypto="Monero"></CardBody>
            </div>
          </div>
        </div>
        <div class="col-3">
          <div class="card mb-3" style="max-width: 18rem">
            <div class="card-body">
              <CardBody :Crypto="Ethereum"></CardBody>
            </div>
          </div>
        </div>
        <div class="col-3">
          <div class="card mb-3" style="max-width: 18rem">
            <div class="card-body">
              <CardBody :Crypto="Litecoin"></CardBody>
            </div>
          </div>
        </div>
      </div>
      <hr />
      <div class="row mt-3">
        <div class="col-md-6 col-sm-3">
          <Chart
            :idChart="idChartBitcoin"
            :labels="this.$store.state.datetimeBitcoin"
            :dataCoin="this.$store.state.priceBitcoin"
            :backgroundColor="backgroundColorBitcoint"
            title="Bitcoin"
          ></Chart>
        </div>
        <div class="col-md-6 col-sm-3">
          <Chart
            :idChart="idChartMonero"
            :labels="this.$store.state.datetimeMonero"
            :dataCoin="this.$store.state.priceMonero"
            :backgroundColor="backgroundColorMonero"
            title="Monero"
          ></Chart>
        </div>

        <div class="col-md-6 col-sm-3">
          <Chart
            :idChart="idChartEthereum"
            :labels="this.$store.state.datetimeEtheurum"
            :dataCoin="this.$store.state.priceEtheurum"
            :backgroundColor="backgroundColorEtheurum"
            title="Ethereum"
          ></Chart>
        </div>
        <div class="col-md-6 col-sm-3">
          <Chart
            :idChart="idChartLitecoin"
            :labels="this.$store.state.datetimeLitecoin"
            :dataCoin="this.$store.state.priceLitecoin"
            :backgroundColor="backgroundColorLitecoin"
            title="Litecoin"
          ></Chart>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
/* eslint-disable */
import { defineAsyncComponent } from "vue";

import axios from "axios";
import CardBody from "./CardBody.vue";
import Chart from "./Chart.vue";
import Loader from "./Loader.vue";

export default {
  name: "Crypt",
  components: {
    CardBody,
    Chart,
    Loader,
  },
  data() {
    return {
      data: null,
      dataBitcoin: "",
      dataMonero: "",
      dataLitecoin: "",
      dataEthereum: "",
      valorEthereum: "",
      valorMonero: "",
      valorLitecoin: "",
      valorBitcoin: "",
      idChartBitcoin: "bitcoin",
      idChartMonero: "monero",
      idChartLitecoin: "litecoin",
      idChartEthereum: "ethereum",
      charge: false,
      backgroundColorEtheurum: "rgb(47, 88, 248)",
      backgroundColorBitcoint: "rgb(229, 176, 24)",
      backgroundColorMonero: "rgb(229, 68, 24)",
      backgroundColorLitecoin: "rgb(49, 57, 0)",
    };
  },
  computed: {
    Bitcoin() {
      if (this.dataBitcoin.data) {
        let found = this.$store.state.datetimeBitcoin.find(
          (element) =>
            element ==
            this.tratamentoDatetime(this.dataBitcoin.data.prices_updated)
        );
        if (typeof found == "undefined") {
          this.$store.state.datetimeBitcoin.push(
            this.tratamentoDatetime(this.dataBitcoin.data.prices_updated)
          );
          this.$store.state.priceBitcoin.push(
            parseFloat(this.dataBitcoin.data.prices.BRL).toFixed(2)
          );
        }
        console.log("Valor:", this.$store.state.priceBitcoin);
        console.log("Data:", this.$store.state.datetimeBitcoin);
        return this.dataBitcoin.data;
      }
    },
    Monero() {
      if (this.dataMonero.data) {
        let found = this.$store.state.datetimeMonero.find(
          (element) =>
            element ==
            this.tratamentoDatetime(this.dataMonero.data.prices_updated)
        );
        if (typeof found == "undefined") {
          this.$store.state.datetimeMonero.push(
            this.tratamentoDatetime(this.dataMonero.data.prices_updated)
          );
          this.$store.state.priceMonero.push(
            parseFloat(this.dataMonero.data.prices.BRL).toFixed(2)
          );
        }
        return this.dataMonero.data;
      }
    },
    Litecoin() {
      if (this.dataLitecoin.data) {
        let found = this.$store.state.datetimeLitecoin.find(
          (element) =>
            element ==
            this.tratamentoDatetime(this.dataLitecoin.data.prices_updated)
        );
        if (typeof found == "undefined") {
          this.$store.state.datetimeLitecoin.push(
            this.tratamentoDatetime(this.dataLitecoin.data.prices_updated)
          );
          this.$store.state.priceLitecoin.push(
            parseFloat(this.dataLitecoin.data.prices.BRL).toFixed(2)
          );
        }
        return this.dataLitecoin.data;
      }
    },
    Ethereum() {
      if (this.dataEthereum.data) {
        let found = this.$store.state.datetimeEtheurum.find(
          (element) =>
            element ==
            this.tratamentoDatetime(this.dataEthereum.data.prices_updated)
        );
        if (typeof found == "undefined") {
          this.$store.state.datetimeEtheurum.push(
            this.tratamentoDatetime(this.dataEthereum.data.prices_updated)
          );
          this.$store.state.priceEtheurum.push(
            parseFloat(this.dataEthereum.data.prices.BRL).toFixed(2)
          );
        }
        return this.dataEthereum.data;
      }
    },
  },
  methods: {
    tratamentoDatetime(datetime) {
      let hour = datetime.split("T")[1].split(":");
      return hour[0] + ":" + hour[1] + ":" + hour[2].split(".")[0];
      //return hour[0] + ":" + hour[1] + ":" + hour[2];
    },
    async getBitcoin() {
      let config = {
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json",
        },
      };

      await axios
        .get("https://api.cryptapi.io/btc/info", config)
        .then((response) => (this.dataBitcoin = response))
        .catch((error) => {
          console.log(error);
        });
    },
    async getEthereum() {
      let config = {
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json",
        },
      };

      await axios
        .get("https://api.cryptapi.io/eth/info", config)
        .then((response) => (this.dataEthereum = response))
        .catch((error) => {
          console.log(error);
        });
    },
    async getLitecoin() {
      let config = {
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json",
        },
      };

      await axios
        .get("https://api.cryptapi.io/ltc/info", config)
        .then((response) => (this.dataLitecoin = response))
        .catch((error) => {
          console.log(error);
        });
    },
    async getMonero() {
      let config = {
        headers: {
          Accept: "application/json",
          "Content-Type": "application/json",
        },
      };

      await axios
        .get("https://api.cryptapi.io/xmr/info", config)
        .then((response) => (this.dataMonero = response))
        .catch((error) => {
          console.log(error);
        });
    },
    getCoin() {
      setTimeout(() => {
        this.getBitcoin();
      }, 1000);
      setTimeout(() => {
        this.getLitecoin();
      }, 2000);
      setTimeout(() => {
        this.getMonero();
      }, 3000);
      setTimeout(() => {
        this.getEthereum();
        this.charge = true;
      }, 4000);
    },
  },
  created() {
    this.getCoin();
    setInterval(() => {
      this.getCoin();
    }, 10000);
  },
};
</script>

<style>
</style>