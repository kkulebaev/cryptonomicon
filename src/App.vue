<template>
    <div class="container mx-auto flex flex-col items-center bg-gray-100 p-4">
        <AppLoader v-if="!isLoading" />
        <div class="container">
            <AddTicker :wallet="wallet" :addTicker="addTicker" :allCoins="allCoins" />

            <CoinCards :wallet="wallet" :select="select" @selectTicker="selectTicker" @deleteTicker="deleteTicker" />

            <DetailCoin :select="select" @closeDetailCoin="select = null" />
        </div>
    </div>
</template>

<script>
import AppLoader from './components/AppLoader.vue'
import AddTicker from './components/AddTicker.vue'
import CoinCards from './components/CoinCards.vue'
import DetailCoin from './components/DetailCoin.vue'
export default {
    data() {
        return {
            isLoading: true,
            wallet: [],
            select: null,
            allCoins: [],
        }
    },
    created() {
        const tickersList = localStorage.getItem('cryptonomicon-wallet')
        if (tickersList) {
            this.wallet = JSON.parse(tickersList)
            this.wallet.forEach((coinItem) => {
                this.updatePriceCoins(coinItem.ticker)
            })
        }

        // Запрос списка всех монет
        const URL = 'https://min-api.cryptocompare.com/data/all/coinlist?summary=true'
        fetch(URL)
            .then((response) => response.json())
            .then(({ Data }) => {
                this.allCoins = Object.keys(Data).map((key) => Data[key].FullName)
            })
    },
    methods: {
        updatePriceCoins(tickerName) {
            const API_KEY = '35943548ed00773f18a8de36c3888518377db9c997e06f5ae99c1256a99f3a03'
            setInterval(async () => {
                const f = await fetch(`https://min-api.cryptocompare.com/data/price?fsym=${tickerName}&tsyms=USD&api_key=${API_KEY}`)
                const data = await f.json()
                this.wallet.find((t) => t.ticker === tickerName).price = data.USD
            }, 10000)
        },
        addTicker(newTicker) {
            this.wallet = [newTicker, ...this.wallet]
            this.updatePriceCoins(newTicker.ticker)
        },
        deleteTicker(id) {
            this.wallet = this.wallet.filter((ticker) => ticker.id !== id)
            if (this.select.id == id) {
                this.select = null
            }
        },
        selectTicker(item) {
            this.select = item
        },
    },
    watch: {
        wallet: function() {
            localStorage.setItem('cryptonomicon-wallet', JSON.stringify(this.wallet))
        },
    },
    components: {
        AppLoader,
        AddTicker,
        CoinCards,
        DetailCoin,
    },
}
</script>

<style>
.img {
    margin: 5px;
}
.size-16 {
    width: 16px;
}
.size-24 {
    width: 24px;
}
.size-32 {
    width: 32px;
}
</style>
