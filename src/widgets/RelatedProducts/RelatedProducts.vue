<script setup lang="ts">
import { computed } from 'vue';
import { useUnit } from 'effector-vue/composition';

import { type Product, ProductList, ProductPreview, getProductByIdQuery } from '@/entities/Product';
import { ProductModel } from '@/entities/Product';

import { AddProductToCartAction } from '@/features/AddProductToCartAction';
import { ProductLikeAction } from '@/features/ProductLikeAction';

const { data: currentProduct } = useUnit(getProductByIdQuery);
const [allProducts] = useUnit([ProductModel.$availableProducts]);

// Находим похожие товары той же категории (исключая текущий товар)
const relatedProducts = computed(() => {
    if (!currentProduct.value?.category) return [];
    
    return allProducts.value
        .filter(product => 
            product.category === currentProduct.value.category && 
            product.id !== currentProduct.value.id
        )
        .slice(0, 4); // Показываем 4 товара
});
</script>

<template>
    <section v-if="relatedProducts.length > 0" class="related-section">
        <div class="related-container">
            <h2 class="related-title">Related Products</h2>
            
            <ProductList class="related-grid">
                <ProductPreview 
                    v-for="product in relatedProducts" 
                    :key="product.id" 
                    :product="product as Product"
                >
                    <template #like-btn>
                        <ProductLikeAction :product-id="product.id" />
                    </template>
                    <template #cart-btn>
                        <AddProductToCartAction :product="product as Product" />
                    </template>
                </ProductPreview>
            </ProductList>
        </div>
    </section>
</template>

<style scoped>
.related-section {
    width: 100%;
    padding: 80px 160px;
    background: #FFFFFF;
}

.related-container {
    max-width: 1440px;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    gap: 32px;
}

.related-title {
    font-family: 'ABeeZee', sans-serif;
    font-weight: 400;
    font-style: italic;
    font-size: 24px;
    line-height: 32px;
    letter-spacing: 0;
    margin: 0;
}

.related-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 16px;
}

/* Адаптивность */
@media (max-width: 768px) {
    .related-section {
        padding: 40px 20px;
    }
    
    .related-grid {
        grid-template-columns: repeat(1, 1fr);
    }
}

@media (max-width: 1024px) {
    .related-grid {
        grid-template-columns: repeat(2, 1fr);
    }
}

@media (max-width: 1200px) {
    .related-section {
        padding: 60px 80px;
    }
    
    .related-grid {
        grid-template-columns: repeat(3, 1fr);
    }
}
</style>
