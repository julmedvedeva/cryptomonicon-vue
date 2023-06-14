<template>
  <div class="container mx-auto flex flex-col items-center bg-gray-100 p-4">
    <!--    <div-->
    <!--      class="fixed w-100 h-100 opacity-80 bg-purple-800 inset-0 z-50 flex items-center justify-center"-->
    <!--    >-->
    <!--      <svg-->
    <!--        class="animate-spin -ml-1 mr-3 h-12 w-12 text-white"-->
    <!--        xmlns="http://www.w3.org/2000/svg"-->
    <!--        fill="none"-->
    <!--        viewBox="0 0 24 24"-->
    <!--      >-->
    <!--        <circle-->
    <!--          class="opacity-25"-->
    <!--          cx="12"-->
    <!--          cy="12"-->
    <!--          r="10"-->
    <!--          stroke="currentColor"-->
    <!--          stroke-width="4"-->
    <!--        ></circle>-->
    <!--        <path-->
    <!--          class="opacity-75"-->
    <!--          fill="currentColor"-->
    <!--          d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"-->
    <!--        ></path>-->
    <!--      </svg>-->
    <!--    </div>-->
    <div class="container">
      <section>
        <div class="flex">
          <div class="max-w-xs">
            <label for="wallet" class="block text-sm font-medium text-gray-700">
              Тикер</label
            >
            <div class="mt-1 relative rounded-md shadow-md">
              <input
                v-model="ticker"
                v-on:keydown.enter="add"
                v-on:input="(e) => findVariables(e.target.value)"
                type="text"
                name="wallet"
                id="wallet"
                class="block w-full pr-10 border-gray-300 text-gray-900 focus:outline-none focus:ring-gray-500 focus:border-gray-500 sm:text-sm rounded-md"
                placeholder="Например DOGE"
              />
            </div>
            <div
              v-if="suggest.length"
              class="flex bg-white shadow-md p-1 rounded-md shadow-md flex-wrap"
            >
              <span
                v-for="s in suggest"
                v-bind:key="s"
                v-on:click="(e) => setTicker(e.target.textContent)"
                class="inline-flex items-center px-2 m-1 rounded-md text-xs font-medium bg-gray-300 text-gray-800 cursor-pointer"
              >
                {{ s }}
              </span>
            </div>
            <div v-if="error" class="text-sm text-red-600">
              Такой тикер уже добавлен
            </div>
          </div>
        </div>
        <button
          v-on:click="add"
          type="button"
          class="my-4 inline-flex items-center py-2 px-4 border border-transparent shadow-sm text-sm leading-4 font-medium rounded-full text-white bg-gray-600 hover:bg-gray-700 transition-colors duration-300 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-500"
        >
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
        <div>
          <button
            type="button"
            v-if="page > 1"
            v-on:click="page = page - 1"
            class="my-4 mr-10 inline-flex items-center py-2 px-4 border border-transparent shadow-sm text-sm leading-4 font-medium rounded-full text-white bg-gray-600 hover:bg-gray-700 transition-colors duration-300 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-500"
          >
            <svg
              class="-ml-0.5 mr-2 h-6 w-6"
              xmlns="http://www.w3.org/2000/svg"
              width="30"
              height="30"
              viewBox="0 0 50 50"
              fill="#ffffff"
            >
              <path
                d="M48.6,23H15.4c-0.9,0-1.3-1.1-0.7-1.7l9.6-9.6c0.6-0.6,0.6-1.5,0-2.1l-2.2-2.2c-0.6-0.6-1.5-0.6-2.1,0
	L2.5,25c-0.6,0.6-0.6,1.5,0,2.1L20,44.6c0.6,0.6,1.5,0.6,2.1,0l2.1-2.1c0.6-0.6,0.6-1.5,0-2.1l-9.6-9.6C14,30.1,14.4,29,15.3,29
	h33.2c0.8,0,1.5-0.6,1.5-1.4v-3C50,23.8,49.4,23,48.6,23z"
              />
            </svg>
            Назад
          </button>
          <button
            v-if="hasNextPage"
            v-on:click="page = page + 1"
            type="button"
            class="my-4 inline-flex items-center py-2 px-4 border border-transparent shadow-sm text-sm leading-4 font-medium rounded-full text-white bg-gray-600 hover:bg-gray-700 transition-colors duration-300 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-500"
          >
            <svg
              class="-ml-0.5 mr-2 h-6 w-6"
              fill="#ffffff"
              xmlns="http://www.w3.org/2000/svg"
              width="30"
              height="30"
              viewBox="0 0 50 50"
            >
              <path
                d="M3.4,29h33.2c0.9,0,1.3,1.1,0.7,1.7l-9.6,9.6c-0.6,0.6-0.6,1.5,0,2.1l2.2,2.2c0.6,0.6,1.5,0.6,2.1,0L49.5,27
	c0.6-0.6,0.6-1.5,0-2.1L32,7.4c-0.6-0.6-1.5-0.6-2.1,0l-2.1,2.1c-0.6,0.6-0.6,1.5,0,2.1l9.6,9.6c0.6,0.7,0.2,1.8-0.7,1.8H3.5
	C2.7,23,2,23.6,2,24.4v3C2,28.2,2.6,29,3.4,29z"
              />
            </svg>
            Вперед
          </button>
        </div>
        <div class="flex">
          <div class="max-w-xs">
            <label for="filter" class="block text-sm font-medium text-gray-700">
              Фильтрация</label
            >
            <div class="mt-1 relative rounded-md shadow-md">
              <input
                v-model="filter"
                type="text"
                id="filter"
                name="filter"
                class="block w-full pr-10 border-gray-300 text-gray-900 focus:outline-none focus:ring-gray-500 focus:border-gray-500 sm:text-sm rounded-md"
                placeholder="Например DOGE"
              />
            </div>
          </div>
        </div>
      </section>
      <template v-if="tickers.length">
        <hr class="w-full border-t border-gray-600 my-4" />
        <dl class="mt-5 grid grid-cols-1 gap-5 sm:grid-cols-3">
          <div
            v-for="t in filteredTickers()"
            v-bind:key="t.name"
            v-on:click="selectTicker(t)"
            v-bind:class="{
              'border-4': sel === t,
            }"
            class="bg-white overflow-hidden shadow rounded-lg border-purple-800 border-solid cursor-pointer"
          >
            <div class="px-4 py-5 sm:p-6 text-center">
              <dt class="text-sm font-medium text-gray-500 truncate">
                {{ t.name }} - USD
              </dt>
              <dd class="mt-1 text-3xl font-semibold text-gray-900">
                {{ t.price }}
              </dd>
            </div>
            <div class="w-full border-t border-gray-200"></div>
            <button
              @click.stop="handleDelete(t)"
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
      </template>
      <section v-if="sel" class="relative">
        <h3 class="text-lg leading-6 font-medium text-gray-900 my-8">
          {{ sel.name }} - USD
        </h3>
        <div class="flex items-end border-gray-600 border-b border-l h-64">
          <div
            v-for="(bar, ind) in normalizeGraph()"
            v-bind:key="ind"
            v-bind:style="{ height: `${bar}%` }"
            class="bg-purple-800 border w-10"
          ></div>
        </div>
        <button
          v-on:click="sel = null"
          type="button"
          class="absolute top-0 right-0"
        >
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="30"
            height="30"
            x="0"
            y="0"
            viewBox="0 0 511.76 511.76"
            style="enable-background: new 0 0 512 512"
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
import { toRaw } from 'vue';

export default {
  name: 'APP',
  data() {
    return {
      ticker: '',
      tickers: [],
      sel: null,
      graph: [],
      allData: null,
      suggest: [],
      error: false,
      filter: '',
      page: 1,
      hasNextPage: true,
    };
  },

  async created() {
    await this.fetchData();

    const tickersData = localStorage.getItem('cryptomonicon-list');
    if (tickersData) {
      this.tickers = JSON.parse(tickersData);
      this.tickers.forEach((ticker) => this.subscribeToUpdate(ticker.name));
    }
  },

  methods: {
    filteredTickers() {
      const start = (this.page - 1) * 6;
      const end = this.page * 6;
      const filtered = this.tickers.filter((ticker) =>
        ticker.name.toLowerCase().includes(this.filter.toLowerCase()),
      );

      this.hasNextPage = filtered.length > end;

      return filtered.slice(start, end);
    },
    async fetchData() {
      const apiKey =
        '645a62f66756dcd69a2fe1a617ee476df5bddab0f1c59c966cb5e70ce7207d20';
      const url = `https://min-api.cryptocompare.com/data/blockchain/list?api_key=${apiKey}`;
      const fetching = await fetch(url);
      const fetchingToJSON = await fetching.json();
      this.allData = fetchingToJSON.Data;
      return (this.allData = fetchingToJSON.Data);
    },
    add() {
      const currentTicker = { name: this.ticker.toUpperCase(), price: '0' };
      this.tickers.push(currentTicker);
      localStorage.setItem('cryptomonicon-list', JSON.stringify(this.tickers));

      this.subscribeToUpdate(currentTicker.name);
      this.ticker = '';
      this.filter = '';
    },
    findVariables(name) {
      this.error = false;
      if (name !== '') {
        const allDataToRaw = Object.keys(toRaw(this.allData));
        const findValueByKey = allDataToRaw.filter((keyword) => {
          return keyword.toLowerCase().includes(name.toLowerCase());
        });

        this.suggest = findValueByKey.slice(0, 4);
      } else {
        this.error = false;
        this.suggest = [];
      }

      return this.suggest;
    },
    setTicker(ticker) {
      this.ticker = ticker;
      const findExistingTicker = toRaw(this.tickers).some((item) => {
        return item.name === ticker;
      });
      if (findExistingTicker === true) {
        this.error = true;
      } else {
        this.error = false;
        this.add();
      }
    },
    subscribeToUpdate(tickerName) {
      setInterval(async () => {
        const fetching = await fetch(
          `https://min-api.cryptocompare.com/data/price?fsym=${tickerName}&tsyms=USD&api_key=645a62f66756dcd69a2fe1a617ee476df5bddab0f1c59c966cb5e70ce7207d20`,
        );
        const dataFetch = await fetching.json();
        // TODO обработчик для ошибки
        // ответ сервера
        // "cccagg_or_exchange market does not exist for this coin pair (MRPH-USD)"
        this.tickers.find((item) => item.name === tickerName).price =
          dataFetch.USD > 1
            ? dataFetch.USD.toFixed(2)
            : dataFetch.USD.toPrecision(2);

        if (this.sel?.name === tickerName) {
          this.graph.push(dataFetch.USD);
        }
      }, 3000);
    },
    selectTicker(ticker) {
      this.sel = ticker;
      this.graph = [];
    },
    handleDelete(t) {
      this.tickers = this.tickers.filter((item) => item !== t);
    },
    normalizeGraph() {
      const maxValue = Math.max(...this.graph);
      const minValue = Math.min(...this.graph);
      return this.graph.map(
        (price) => 5 + ((price - minValue) * 95) / (maxValue - minValue),
      );
    },
  },
  watch: {
    filter() {
      this.page = 1;
    },
  },
};
</script>

<style src="./app.css"></style>
