<script setup lang="ts">
defineProps<{
    label: string;
    icon: string;
    category: string;
}>();
</script>

<template>
    <li class="root">
        <RouterLink :to="{ path: '/products', query: { category } }" class="card-link">
            <span :class="icon" />
            <p>
                {{ label }}
            </p>
        </RouterLink>
    </li>
</template>

<style scoped>
.root {
    display: flex;
    flex-direction: column;
    border-radius: 1rem;
    font-size: 16px;
    text-align: center;
    background-color: #ededed;
    transition: all 0.3s ease;

    &:hover {
        background-color: #d0d0d0;
        transform: translateY(-2px);
    }

    @media (width >= 1200px) {
        width: 100%;
        max-width: 160px;
    }
}

.card-link {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    padding: 24px;
    gap: 8px;
    width: 100%;
    height: 100%;
    color: inherit;
    text-decoration: none;

    span {
        font-size: 48px;
    }
}
</style>
