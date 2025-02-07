<!-- eslint-disable vue/multi-word-component-names -->
<template>
    <div>
        <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-50"></div>
        <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
            <DrawerHead />
            <CartItemList />

            <div class="flex-col flex gap-4 mb-6 mt-7">
                <div class="flex gap-2">
                    <span>Итого: </span>
                    <div class="flex-1 border-b border-dashed"></div>
                    <b>21 498 руб. </b>
                </div>

                <div class="flex gap-2">
                    <span>Налог 5%: </span>
                    <div class="flex-1 border-b border-dashed"></div>
                    <b>1074 руб.  </b>
                </div>

                <button
                    disabled="" 
                    class="mt-4 transition bg-lime-500 w-full rounded-xl py-3 text-white hover:bg-lime-600 disabled:bg-slate-300 active:bg-lime-700 cursor-pointer">
                    Офрмить заказ
                </button>
            </div>

        </div>
    </div>
</template>

<script setup>
    import DrawerHead from './DrawerHead.vue';
    import CartItemList from './CartItemList.vue';
</script>

<style scoped>

</style>