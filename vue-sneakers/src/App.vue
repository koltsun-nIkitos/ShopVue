<template>
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Drawer v-if="drawerOpen"/>

    <Header @open-drawer="openDrawer"/>

    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">Все кроссовки</h2>

        <div class="flex gap-4">
          <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
            <option value="name">По названию</option>
            <option value="price">По цене (дешевые)</option>
            <option value="-price">По цене (дорогие)</option>
          </select>

          <div class="relative">
            <img class="absolute left-4 top-3" src="/search.svg" alt="search icon" />
            <input
              @change="onChangeSearchInput"
              class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
              type="text"
              placeholder="Поиск..."
            />
          </div>
        </div>
      </div>

      <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="addToCart"/>
    </div>
  </div>
</template>

<script setup>
import { onMounted, ref, reactive, watch, provide } from 'vue';
import axios from 'axios';

import Drawer from './components/Drawer.vue'
import Header from './components/Header.vue'
import CardList from './components/CardList.vue'

const items = ref([]);
const cart = ref([]);

const drawerOpen = ref(false);

const closeDrawer = () =>{
  drawerOpen.value = false;
};

const openDrawer = () =>{
  drawerOpen.value = true;
};

const filters = reactive({
  sortBy : 'title',
  searchQuerry : '',
});

const fetchFavorites = async () =>{
  try{
    const {data : favorites} = await axios.get('https://573ada8fd6c965d4.mokky.dev/favorites');

    items.value = items.value.map(item => {
      const favorite = favorites.find(favorite => favorite.parentId === item.id);

      if (!favorite){
        return item;
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id,
      }
    });
  }
  catch(err){
    console.log(err)
  }
};

const addToFavorite = async (item) =>{
  try{

    if (!item.isFavorite){
      const obj ={
        parentId: item.id
      };
    
      item.isFavorite = true;

      const data = await axios.post('https://573ada8fd6c965d4.mokky.dev/favorites', obj);
      item.favoriteId = data.id;

    }else{
      item.isFavorite = false;

      await axios.delete(`https://573ada8fd6c965d4.mokky.dev/favorites/${item.favoriteId}`);
      item.favoriteId = null;
    }

  }catch (err){
    console.log(err);
  }
}

const addToCart = (item) =>{
  if(!item.isAdded){
    cart.value.push(item);
    item.isAdded = true;
  }else{
    cart.value.splice(cart.value.indexOf(item), 1);
    item.isAdded = false;
  } 

}

const onChangeSelect = event =>{
  filters.sortBy = event.target.value;
}

const onChangeSearchInput = event =>{
  filters.searchQuerry = event.target.value;
}

const fetchItems = async ()=>{
  try{
    const params = {
      sortBy: filters.sortBy
    };

    if(filters.searchQuerry){
      params.title = `*${filters.searchQuerry}*`;
    }

    const {data} = await axios.get('https://573ada8fd6c965d4.mokky.dev/items', {
      params
    });

    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }));
  }
  catch(err){
    console.log(err)
  }
};

onMounted(async () => {
  await fetchItems();
  await fetchFavorites();
});

watch(filters, fetchItems);
provide('cart', {
  cart,
  closeDrawer, 
  openDrawer
})

</script>

<style scoped></style>
