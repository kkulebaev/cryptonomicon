<template>
    <dl class="mt-5 grid grid-cols-1 gap-5 sm:grid-cols-3">
        <CoinCard v-for="item in wallet" :item="item" :select="select" :key="item.id" @selectTicker="$emit('selectTicker', item)" @deleteTicker="$emit('deleteTicker', item.id)" />
    </dl>
</template>

<script>
import CoinCard from './CoinCard.vue'
export default {
    emits: ['selectTicker', 'deleteTicker'],
    props: {
        wallet: {
            type: Array,
            required: true,
        },
        select: {
            type: Object,
            required: false,
            default: null,
        },
    },
    components: {
        CoinCard,
    },
}
</script>
