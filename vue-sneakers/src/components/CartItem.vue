<template>
    <div class="border border-slate-200 p-4 rounded-xl flex items-center gap-4">
        <img :src="imageUrl" :alt="title" class="w-16 h-16">

        <div class="flex flex-col">
            <p>{{ title }}</p>

            <div class="flex justify-between mt-2">
                <b>{{ price }} руб.</b>
                <img class="opacity-40 hover:opacity-100 cursor-pointer transition" 
                    src="/close.svg"
                    alt="close icon"
                >
            </div>
        </div>

    </div>
</template>

<script setup>
    defineProps({
        id: Number,
        title: String,
        imageUrl: String,
        price: Number,
        
    })
</script>