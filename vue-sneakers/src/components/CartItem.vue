<template>
    <div class="border border-slate-200 p-4 rounded-xl flex items-center gap-4">
        <img :src="imageUrl" :alt="title" class="w-16 h-16">

        <div class="flex flex-col flex-1">
            <p>{{ title }}</p>

            <div class="flex justify-between mt-2">
                <b class="flex-1">{{ price }} руб.</b>
                <img @click="emit('onClickRemove')" class="opacity-40 hover:opacity-100 cursor-pointer transition" 
                    src="/close.svg"
                    alt="close icon"
                >
            </div>
        </div>

    </div>
</template>

<script setup>
    const emit = defineEmits('onClickRemove')

    defineProps({
        id: Number,
        title: String,
        imageUrl: String,
        price: Number,
        
    })
</script>