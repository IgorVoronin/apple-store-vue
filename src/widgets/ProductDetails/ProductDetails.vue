<script setup lang="ts">
import { useUnit } from 'effector-vue/composition';

import { getProductByIdQuery } from '@/entities/Product';

const { data: product } = useUnit(getProductByIdQuery);
</script>

<template>
    <section class="details-section">
        <div class="details-container">
            <div class="details-content">
                <h2 class="details-title">Details</h2>
                
                <!-- Описание товара (если есть) -->
                <p class="details-description">
                    Just as a book is judged by its cover, the first thing you notice when you pick up a modern smartphone is the display. Nothing surprising, because advanced
                    technologies allow you to practically level the display frames and cutouts for the front camera and speaker, making it look like a solid piece of glass. And how
                    good that in such routes both Apple and Samsung are the pioneers. Both companies last year for the first time in the world, release aphone with a giant screen, while
                    the body is made very compact. Also from the reviews is the camera is just a blessing, clean pictures, every smallest detail. And selfie is just super. The phone is
                    very light yet premium. And the larger the screen, the more space is available for the content placed on it.
                </p>

                <!-- Характеристики товара -->
                <div class="characteristics-grid">
                    <div 
                        v-for="(char, index) in product.characteristics" 
                        :key="index" 
                        class="characteristic-row"
                    >
                        <span class="characteristic-label">{{ char.characteristic }}</span>
                        <span class="characteristic-value">{{ char.value }} {{ char.unit_type !== 'значение' ? char.unit_type : '' }}</span>
                    </div>
                </div>
            </div>
        </div>
    </section>
</template>

<style scoped>
.details-section {
    width: 100%;
    background: #FAFAFA;
    padding: 80px 160px;
}

.details-container {
    max-width: 1440px;
    margin: 0 auto;
}

.details-content {
    background: #FFFFFF;
    border-radius: 8px;
    padding: 48px 40px;
    display: flex;
    flex-direction: column;
    gap: 32px;
}

.details-title {
    font-family: 'ABeeZee', sans-serif;
    font-weight: 400;
    font-style: italic;
    font-size: 20px;
    line-height: 24px;
    letter-spacing: 0;
    margin: 0;
}

.details-description {
    font-family: 'ABeeZee', sans-serif;
    font-weight: 400;
    font-style: italic;
    font-size: 14px;
    line-height: 24px;
    letter-spacing: 0;
    color: #6C6C6C;
    margin: 0;
}

.characteristics-grid {
    display: flex;
    flex-direction: column;
    gap: 16px;
}

.characteristic-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding-bottom: 16px;
    border-bottom: 1px solid #EDEDED;
}

.characteristic-row:last-child {
    border-bottom: none;
}

.characteristic-label {
    font-family: 'Abel', sans-serif;
    font-weight: 400;
    font-style: normal;
    font-size: 16px;
    line-height: 24px;
    letter-spacing: 0;
    color: #6C6C6C;
}

.characteristic-value {
    font-family: 'Abel', sans-serif;
    font-weight: 400;
    font-style: normal;
    font-size: 16px;
    line-height: 24px;
    letter-spacing: 0;
    color: #000000;
    text-align: right;
}

/* Адаптивность для мобильных */
@media (max-width: 768px) {
    .details-section {
        padding: 40px 20px;
    }
    
    .details-content {
        padding: 24px 20px;
    }
}

@media (max-width: 1200px) {
    .details-section {
        padding: 60px 80px;
    }
}
</style>
