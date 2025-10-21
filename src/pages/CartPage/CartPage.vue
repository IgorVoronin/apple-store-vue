<script setup lang="ts">
import { useUnit } from 'effector-vue/composition';

import { API } from '@/shared/api';
import { normalNumberFormat, shuffled } from '@/shared/lib';
import { MainContainer } from '@/shared/ui';

import { ProductModel } from '@/entities/Product';

import { RootLayout } from '@/widgets/RootLayout';

const [cart, totalCartPrice] = useUnit([ProductModel.$productCart, ProductModel.$totalCartPrice]);

const [onAddProductToCart, onRemoveProductFromCart, onDeleteProduct, resetCart] = useUnit([
    ProductModel.cartProductSettled,
    ProductModel.cartProductRemoved,
    ProductModel.cartProductDelete,
    ProductModel.cartResettled,
]);

const handleCheckout = () => {
    localStorage.setItem('cart-products', JSON.stringify({}));
    resetCart();
};
</script>
<template>
    <RootLayout>
        <MainContainer class="py-10 lg:grid lg:grid-cols-2 lg:gap-[48px]">
            <div>
                <h2 class="text-lg mb-10">Shopping Cart</h2>
                <ul class="py-10 grid gap-10">
                    <li
                        v-for="{ product } in Object.values(cart)"
                        :key="product.id"
                        class="not-last-of-type:border-b border-solid border-gray-300"
                    >
                        <RouterLink :to="`/product/${product.id}`">
                            <div class="pb-10 flex gap-4 items-center">
                                <Image width="90" :src="API.BASE_URL + shuffled([...product.images])[0]" />
                                <div>
                                    <p style="font-family: 'ABeeZee', sans-serif; font-weight: 400; font-style: italic; font-size: 16px; line-height: 24px;">{{ product.name }}</p>
                                    <p># {{ product.id }}</p>
                                    <div class="flex items-center gap-6">
                                        <div class="flex gap-2 items-center">
                                            <Button
                                                severity="contrast"
                                                text
                                                size="large"
                                                class="text-xl"
                                                @click.stop.prevent="onRemoveProductFromCart(Object.assign(product))"
                                                >–</Button
                                            >
                                            <div
                                                class="border w-[32px] text-center h-[32px] border-gray-100 leading-[1.85] rounded-md"
                                            >
                                                {{ cart[product.id]?.count }}
                                            </div>
                                            <Button
                                                severity="contrast"
                                                text
                                                size="large"
                                                class="text-xl"
                                                @click.stop.prevent="onAddProductToCart(Object.assign(product))"
                                                >+</Button
                                            >
                                        </div>
                                        <p style="font-family: 'ABeeZee', sans-serif; font-weight: 400; font-style: italic; font-size: 20px; line-height: 32px; letter-spacing: 0.03em;">{{ normalNumberFormat(cart[product.id]?.product.price) }}</p>
                                        <Button
                                            text
                                            severity="contrast"
                                            icon="pi pi-times"
                                            @click.stop.prevent="onDeleteProduct(Object.assign(product))"
                                        />
                                    </div>
                                </div>
                            </div>
                        </RouterLink>
                    </li>
                </ul>
            </div>
            <div class="border py-[56px] px-4 rounded-lg border-gray-200 flex flex-col">
                <h2 class="mb-10" style="font-family: 'ABeeZee', sans-serif; font-weight: 400; font-style: italic; font-size: 20px; line-height: 16px;">Order Summary</h2>
                
                <!-- Discount code -->
                <div class="mb-6">
                    <label class="text-gray-600 mb-2 block" style="font-family: 'ABeeZee', sans-serif; font-weight: 400; font-style: italic; font-size: 14px; line-height: 16px;">Discount code / Promo code</label>
                    <input 
                        type="text" 
                        placeholder="Code" 
                        class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:outline-none focus:border-gray-400"
                    />
                </div>
                
                <!-- Bonus card -->
                <div class="mb-10">
                    <label class="text-gray-600 mb-2 block" style="font-family: 'ABeeZee', sans-serif; font-weight: 400; font-style: italic; font-size: 14px; line-height: 16px;">Your bonus card number</label>
                    <div class="relative">
                        <input 
                            type="text" 
                            placeholder="Enter Card Number" 
                            class="w-full px-4 py-3 pr-20 border border-gray-300 rounded-lg focus:outline-none focus:border-gray-400"
                        />
                        <button class="absolute right-2 top-1/2 -translate-y-1/2 px-4 py-1.5 border border-black rounded-lg text-sm hover:bg-gray-50">
                            Apply
                        </button>
                    </div>
                </div>
                
                <!-- Price breakdown -->
                <div class="space-y-4 mb-6">
                    <div class="flex justify-between">
                        <p class="font-semibold" style="font-family: 'ABeeZee', sans-serif; font-weight: 400; font-style: italic; font-size: 16px; line-height: 24px; letter-spacing: 0.03em;">Subtotal</p>
                        <p class="font-semibold" style="font-family: 'ABeeZee', sans-serif; font-weight: 400; font-style: italic; font-size: 16px; line-height: 32px; letter-spacing: 0.03em;">{{ normalNumberFormat(totalCartPrice) }}</p>
                    </div>
                    <div class="flex justify-between">
                        <p class="text-gray-600">Estimated Tax</p>
                        <p class="font-medium" style="font-family: 'ABeeZee', sans-serif; font-weight: 400; font-style: italic; font-size: 16px; line-height: 32px; letter-spacing: 0.03em;">{{ normalNumberFormat(Math.round(totalCartPrice * 0.02)) }}</p>
                    </div>
                    <div class="flex justify-between">
                        <p class="text-gray-600">Estimated shipping & Handling</p>
                        <p class="font-medium" style="font-family: 'ABeeZee', sans-serif; font-weight: 400; font-style: italic; font-size: 16px; line-height: 32px; letter-spacing: 0.03em;">{{ normalNumberFormat(totalCartPrice > 0 ? 29 : 0) }}</p>
                    </div>
                </div>
                
                <!-- Total -->
                <div class="flex justify-between mb-10 pt-4 border-t border-gray-200">
                    <p class="font-semibold" style="font-family: 'ABeeZee', sans-serif; font-weight: 400; font-style: italic; font-size: 16px; line-height: 24px; letter-spacing: 0.03em;">Total</p>
                    <p class="font-semibold" style="font-family: 'ABeeZee', sans-serif; font-weight: 400; font-style: italic; font-size: 16px; line-height: 32px; letter-spacing: 0.03em;">{{ normalNumberFormat(totalCartPrice + Math.round(totalCartPrice * 0.02) + (totalCartPrice > 0 ? 29 : 0)) }}</p>
                </div>
                
                <!-- Checkout button -->
                <Button severity="contrast" size="large" class="w-full mt-auto" @click="handleCheckout">
                    Checkout
                </Button>
            </div>
        </MainContainer>
    </RootLayout>
</template>
