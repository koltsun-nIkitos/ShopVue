<template>
    <div class="grid grid-cols-4 gap-5" v-auto-animate>
        <Card 
            v-for="item in items"
            :key="item.id"
            :id="item.id"
            :title="item.title" 
            :image-url="item.imageUrl" 
            :price="item.price"
            :onClickFavorite="isFavorite ? null : () => emit('addToFavorite', item)"
            :onClickAdd="isFavorite ? null : () => emit('addToCart', item)"
            :isFavorite="item.isFavorite"
            :isAdded="item.isAdded"
        />
        
    </div>
</template>

<script setup>
    import Card from './Card.vue';

    const emit = defineEmits(['addToFavorite', 'addToCart']);

    defineProps({
        items: Array,
        isFavorite: Boolean
    });



</script>