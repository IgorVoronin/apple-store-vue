<script setup lang="ts">
import { watch } from 'vue';
import { useRoute } from 'vue-router';
import { useUnit } from 'effector-vue/composition';

import { MainContainer } from '@/shared/ui';

import { getAllProductsQuery, getProductByIdQuery } from '@/entities/Product';

import { ProductGallery } from '@/widgets/ProductGallery';
import { ProductInfo } from '@/widgets/ProductInfo';
import { ProductDetails } from '@/widgets/ProductDetails';
import { RelatedProducts } from '@/widgets/RelatedProducts';
import { RootLayout } from '@/widgets/RootLayout';

const { pending } = useUnit(getAllProductsQuery);

const route = useRoute();

// При смене ID товара загружаем новый и скроллим вверх
watch(
    () => route.params.id as string,
    (id) => {
        getProductByIdQuery.start(id);
        // Скролл вверх страницы
        window.scrollTo({ top: 0, behavior: 'smooth' });
    },
    { immediate: true }
);
</script>
<template>
    <RootLayout>
        <ProgressSpinner v-if="pending" class="m-auto flex mt-[20%]" />
        <div v-if="!pending">
            <MainContainer class="py-10">
                <div class="lg:flex lg:gap-[48px]">
                    <ProductGallery />
                    <ProductInfo />
                </div>
            </MainContainer>
            <ProductDetails />
            <RelatedProducts />
        </div>
    </RootLayout>
</template>
