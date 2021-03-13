<template>
    <section>
        <div class="flex">
            <div class="max-w-xs">
                <label for="wallet" class="block text-sm font-medium text-gray-700">Тикер</label>
                <div class="mt-1 relative rounded-md shadow-md">
                    <input
                        v-model.trim="inputValue"
                        @keydown.enter="addCoinHandler"
                        type="text"
                        name="wallet"
                        id="wallet"
                        class="p-3 block w-full pr-10 border-gray-300 text-gray-900 focus:outline-none focus:ring-gray-500 focus:border-gray-500 sm:text-sm rounded-md"
                        placeholder="Например BTC"
                    />
                </div>
                <div v-if="inputValue" class="flex bg-white shadow-md p-1 rounded-md shadow-md flex-wrap">
                    <span
                        v-for="(item, index) in possibleTickers"
                        :key="index"
                        @click="this.inputValue = item"
                        class="inline-flex items-center px-2 m-1 rounded-md text-xs font-medium bg-gray-300 text-gray-800 cursor-pointer"
                    >
                        {{ item }}
                    </span>
                </div>
                <div v-if="isAdded" class="text-sm text-red-600">Такой тикер уже добавлен</div>
            </div>
        </div>
        <button
            @click="addCoinHandler"
            type="button"
            class="my-4 inline-flex items-center py-2 px-4 border border-transparent shadow-sm text-sm leading-4 font-medium rounded-full text-white bg-gray-600 hover:bg-gray-700 transition-colors duration-300 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-500"
        >
            <img class="img size-24" src="./../assets/img/add.svg" alt="add" />
            Добавить
        </button>
    </section>
</template>

<script>
export default {
    props: {
        wallet: {
            type: Array,
            required: true,
        },
        addTicker: {
            type: Function,
            required: true,
        },
        allCoins: {
            type: Array,
            required: true,
        },
    },
    data() {
        return {
            inputValue: '',
            possibleTickers: [],
            isAdded: false,
        }
    },
    methods: {
        addCoinHandler: function() {
            if (this.inputValue) {
                const searchEl = this.wallet.find((coinItem) => {
                    return coinItem.ticker === this.inputValue
                })
                if (searchEl == undefined) {
                    const newTicker = { id: Date.now(), ticker: this.inputValue, price: '-' }
                    this.addTicker(newTicker)
                    this.inputValue = ''
                } else {
                    this.isAdded = true
                }
            }
        },
    },
    watch: {
        inputValue: function() {
            if (this.inputValue) {
                this.possibleTickers = this.allCoins.filter((coinName) => coinName.includes(this.inputValue))
                this.possibleTickers.splice(4, this.possibleTickers.length - 4)
            } else {
                this.possibleTickers = null
            }
            this.isAdded = false
        },
    },
}
</script>
