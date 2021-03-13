<template>
    <div
        :class="{ 'border-purple-800': select === item }"
        class="bg-white overflow-hidden shadow rounded-lg border-transparent border-4 border-solid cursor-pointer"
        @click="$emit('selectTicker', item)"
    >
        <div class="px-4 py-5 sm:p-6 text-center">
            <dt class="text-sm font-medium text-gray-500 truncate">{{ item.ticker }} - USD</dt>
            <dd class="mt-1 text-3xl font-semibold text-gray-900">
                {{ item.price }}
            </dd>
        </div>
        <div class="w-full border-t border-gray-200"></div>
        <button
            @click.stop="$emit('deleteTicker', item.id)"
            class="flex items-center justify-center font-medium w-full bg-gray-100 px-4 py-4 sm:px-6 text-md text-gray-500 hover:text-gray-600 hover:bg-gray-200 hover:opacity-20 transition-all focus:outline-none"
        >
            <img class="img size-16" src="./../assets/img/delete.svg" alt="delete" />
            Удалить
        </button>
    </div>
</template>

<script>
export default {
    emits: ['selectTicker', 'deleteTicker'],
    props: {
        item: {
            type: Object,
            required: true,
        },
        select: {
            type: Object,
            required: false,
            default: null,
        },
    },
}
</script>
