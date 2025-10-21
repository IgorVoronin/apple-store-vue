<script setup lang="ts">
import { onMounted, ref, watch } from 'vue';
import { useRoute } from 'vue-router';
import { useUnit, useVModel } from 'effector-vue/composition';

import { isLargeScreen } from '@/shared/lib';
import { MainContainer } from '@/shared/ui';

import { type Product, ProductList, ProductModel, ProductPreview } from '@/entities/Product';

import { AddProductToCartAction } from '@/features/AddProductToCartAction';
import { ProductFilters, ProductFiltersModel } from '@/features/ProductFilters';
import { ProductLikeAction } from '@/features/ProductLikeAction';

import { ProductsGridFilterBar } from '@/widgets/ProductsGrid';

const route = useRoute();
const [products] = useUnit([ProductFiltersModel.$paginatedProducts]);
const selectedCategories = useVModel(ProductModel.$selectedCategories);
const selectedProcessors = useVModel(ProductModel.$selectedProcessors);
const selectedDiagonals = useVModel(ProductModel.$selectedDiagonals);

const page = ref(0);

// Сбрасываем все фильтры и устанавливаем категорию из URL
onMounted(() => {
    const category = route.query.category as string;
    // Сбрасываем все фильтры
    selectedProcessors.value = [];
    selectedDiagonals.value = [];
    
    if (category) {
        selectedCategories.value = [category];
    } else {
        selectedCategories.value = [];
    }
});

// Отслеживаем изменения URL
watch(() => route.query.category, (newCategory) => {
    // Сбрасываем все фильтры при смене категории
    selectedProcessors.value = [];
    selectedDiagonals.value = [];
    
    if (newCategory) {
        selectedCategories.value = [newCategory as string];
    } else {
        selectedCategories.value = [];
    }
    page.value = 0; // Сбрасываем на первую страницу
});
</script>

<template>
    <MainContainer class="container">
        <div class="lg:grid grid-cols-[256px_1fr] lg:gap-[32px]">
            <ProductFilters v-if="isLargeScreen" />

            <div>
                <ProductsGridFilterBar />
                <ProductList class="list">
                    <ProductPreview v-for="product in products[page]" :key="product.id" :product="product as Product">
                        <template #like-btn>
                            <ProductLikeAction :product-id="product.id" />
                        </template>
                        <template #cart-btn>
                            <AddProductToCartAction :product="product as Product" />
                        </template>
                    </ProductPreview>
                </ProductList>
            </div>
        </div>

        <Paginator v-model:first="page" :rows="1" :total-records="products.length" />
    </MainContainer>
</template>
<style scoped>
.container {
    margin-top: 40px;
    margin-bottom: 72px;
}

.list {
    @media (width >= 1200px) {
        grid-template-columns: repeat(3, 1fr);
    }
}
</style>
