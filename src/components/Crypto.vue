<template>
  <div class="container mx-auto flex flex-col items-center bg-gray-100 p-4">
    <spiner v-bind:isShow="false"></spiner>
    <div class="container">
      <section>
        <div class="flex">
          <div class="max-w-xs">
            <label for="wallet" class="block text-sm font-medium text-gray-700"
              >Тикер</label
            >
            <div class="mt-1 relative rounded-md shadow-md">
              <input
                v-model="newTiket"
                v-on:keypress="clearError"
                v-on:keypress.enter="searchTiket"
                type="text"
                name="wallet"
                id="wallet"
                class="block w-full pr-10 border-gray-300 text-gray-900 focus:outline-none focus:ring-gray-500 focus:border-gray-500 sm:text-sm rounded-md"
                placeholder="Например DOGE"
              />
            </div>
            <div
              class="flex bg-white shadow-md p-1 rounded-md shadow-md flex-wrap"
            >
            @todo
              <span
                class="inline-flex items-center px-2 m-1 rounded-md text-xs font-medium bg-gray-300 text-gray-800 cursor-pointer"
              >
                BTC
              </span>
              <span
                class="inline-flex items-center px-2 m-1 rounded-md text-xs font-medium bg-gray-300 text-gray-800 cursor-pointer"
              >
                DOGE
              </span>
              <span
                class="inline-flex items-center px-2 m-1 rounded-md text-xs font-medium bg-gray-300 text-gray-800 cursor-pointer"
              >
                BCH
              </span>
              <span
                class="inline-flex items-center px-2 m-1 rounded-md text-xs font-medium bg-gray-300 text-gray-800 cursor-pointer"
              >
                CHD
              </span>
            </div>
            <div class="text-sm text-red-600">
              {{error}}
            </div>
          </div>
        </div>
        <button
          type="button"
          v-on:click="searchTiket"
          class="my-4 inline-flex items-center py-2 px-4 border border-transparent shadow-sm text-sm leading-4 font-medium rounded-full text-white bg-gray-600 hover:bg-gray-700 transition-colors duration-300 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-500"
        >
          <!-- Heroicon name: solid/mail -->
          <svg
            class="-ml-0.5 mr-2 h-6 w-6"
            xmlns="http://www.w3.org/2000/svg"
            width="30"
            height="30"
            viewBox="0 0 24 24"
            fill="#ffffff"
          >
            <path
              d="M13 7h-2v4H7v2h4v4h2v-4h4v-2h-4V7zm-1-5C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.41 0-8-3.59-8-8s3.59-8 8-8 8 3.59 8 8-3.59 8-8 8z"
            ></path>
          </svg>
          Добавить
        </button>
      </section>

      <hr class="w-full border-t border-gray-600 my-4" />

      <dl class="mt-5 grid grid-cols-1 gap-5 sm:grid-cols-3">
        <div
          v-for="tiker of tikerList"
          :key="tiker.name"
          class="bg-white overflow-hidden shadow rounded-lg border-purple-800 border-solid cursor-pointer"
          :class="{'border-4': isSelected(tiker.name)}"
        >
          <div
            @click="select(tiker.name)"
            class="px-4 py-5 sm:p-6 text-center">
            <dt class="text-sm font-medium text-gray-500 truncate">
              {{tiker.name}} - USD
            </dt>
            <dd class="mt-1 text-3xl font-semibold text-gray-900">
              {{tiker.value}} $
            </dd>
          </div>
          <div class="w-full border-t border-gray-200"></div>
          <button
            @click="removeTiketByName(tiker.name)"
            class="flex items-center justify-center font-medium w-full bg-gray-100 px-4 py-4 sm:px-6 text-md text-gray-500 hover:text-gray-600 hover:bg-gray-200 hover:opacity-20 transition-all focus:outline-none"
          >
            <svg
              class="h-5 w-5"
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 20 20"
              fill="#718096"
              aria-hidden="true"
            >
              <path
                fill-rule="evenodd"
                d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z"
                clip-rule="evenodd"
              ></path></svg
            >Удалить
          </button>
        </div>
      </dl>

      <hr class="w-full border-t border-gray-600 my-4" />

      <section
        v-if="selected"
        class="relative">
        <h3 class="text-lg leading-6 font-medium text-gray-900 my-8">
          {{selected}} - USD
        </h3>
        <div class="flex items-end border-gray-600 border-b border-l h-64">
          <div
            v-for="(val, index) of normilizeGraph()"
            :key="index"
            class="bg-purple-800 border w-10" :style="{'height': val+'%'}"></div>
        </div>
        <button
          @click="select(null)"
          type="button"
          class="absolute top-0 right-0">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink"
            xmlns:svgjs="http://svgjs.com/svgjs"
            version="1.1"
            width="30"
            height="30"
            x="0"
            y="0"
            viewBox="0 0 511.76 511.76"
            style="enable-background:new 0 0 512 512"
            xml:space="preserve"
          >
            <g>
              <path
                d="M436.896,74.869c-99.84-99.819-262.208-99.819-362.048,0c-99.797,99.819-99.797,262.229,0,362.048    c49.92,49.899,115.477,74.837,181.035,74.837s131.093-24.939,181.013-74.837C536.715,337.099,536.715,174.688,436.896,74.869z     M361.461,331.317c8.341,8.341,8.341,21.824,0,30.165c-4.16,4.16-9.621,6.251-15.083,6.251c-5.461,0-10.923-2.091-15.083-6.251    l-75.413-75.435l-75.392,75.413c-4.181,4.16-9.643,6.251-15.083,6.251c-5.461,0-10.923-2.091-15.083-6.251    c-8.341-8.341-8.341-21.845,0-30.165l75.392-75.413l-75.413-75.413c-8.341-8.341-8.341-21.845,0-30.165    c8.32-8.341,21.824-8.341,30.165,0l75.413,75.413l75.413-75.413c8.341-8.341,21.824-8.341,30.165,0    c8.341,8.32,8.341,21.824,0,30.165l-75.413,75.413L361.461,331.317z"
                fill="#718096"
                data-original="#000000"
              ></path>
            </g>
          </svg>
        </button>
      </section>
    </div>
  </div>
</template>

<script>

import Spiner from './Spiner.vue';

export default {
  components: { Spiner },
  name: "Crypro",
  data() {
    return {
      newTiket: "",
      tikerList: [],
      selected: null,
      graph: [],
      error: null,
    };
  },
  created: function () {
    setInterval(this.update, 5000);
  },
  methods: {
    update: function() {
      if (!this.tikerList) {
        return false;
      }
      this.tikerList.forEach(async (tiker) => {
        let self = this;
        await fetch(`https://min-api.cryptocompare.com/data/price?fsym=${tiker.name}&tsyms=USD&api_key=9b53fb3b2daab7f7df34d09bcf8b32266a23d799a8e23af20b601c381e349161`)
          .then(response => {
            if (response.status == 200) {
              return response.json();
            }
            throw Error("Сервер не отвечает");
          })
          .then(exchange => {
            if (exchange.USD) {
              tiker.value = exchange.USD;
              if (self.selected == tiker.name) {
                self.graph.push(exchange.USD);
              }
            } else {
              throw Error("Токен не найден");
            }
          })
          .catch(err => {
            self.error = err;
          });
      });
    },
    searchTiket: async function() {
      let self = this;
      if (this.isValidTiket()) {
        await fetch(`https://min-api.cryptocompare.com/data/price?fsym=${this.newTiket}&tsyms=USD&api_key=9b53fb3b2daab7f7df34d09bcf8b32266a23d799a8e23af20b601c381e349161`)
          .then(response => {
            if (response.status == 200) {
              return response.json();
            }
            throw Error("Сервер не отвечает");
          })
          .then(exchange => {
            if (exchange.USD) {
              self.tikerList.push({
                name: self.newTiket.toUpperCase(),
                value: exchange.USD,
              });
              self.newTiket = "";
            } else {
              throw Error("Токен не найден");
            }
          })
          .catch(err => {
            self.error = err;
          });
      }
    },
    isValidTiket: function() {
      if (this.newTiket == "") {
        this.error = "Введите тикер";
        return false;
      }
      if (this.tikerList.some(tiker => tiker.name == this.newTiket.toUpperCase())) {
        this.error = "Такой тикер уже добавлен";
        return false;
      }
      return true;
    },
    removeTiketByName: function(name) {
      if (this.selected == name) {
        this.select(null);
      }
      this.tikerList = this.tikerList.filter(tiker => tiker.name !== name);
    },
    select: function(name) {
      if (this.selected != name) {
        this.selected = name;
        this.graph = [];
      }
    },
    isSelected: function(name) {
      return this.selected == name;
    },
    clearError: function() {
      this.error = null;
    },
    normilizeGraph: function() {
      let min = Math.min(...this.graph);
      let max = Math.max(...this.graph);
      let interval = max == min ? 1 : max - min;
      return this.graph.map(function (val) {
          return Math.round( (val - min) / interval * 90 + 5 );
      });
    }
  }
};
</script>
