<template>
    <div class="flex items-center">
            <div class="flex items-center">
                <label v-for="star in maxStars" :key="star" class="flex items-center mr-1">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" viewBox="0 0 24 24">
                        <path v-if="star <= rating" fill="#f1c40f" d="M12 1l3.09 6.3L22 9.27l-5.54 5.4L18.72 23 12 19.4 5.28 23l1.26-8.33L2 9.27l6.91-1.97z" />
                        <path v-else fill="#d8d8d8" d="M12 1l3.09 6.3L22 9.27l-5.54 5.4L18.72 23 12 19.4 5.28 23l1.26-8.33L2 9.27l6.91-1.97z" />
                    </svg>
                </label>
                <p class="ms-1 text-sm font-medium text-gray-500 dark:text-gray-400">{{rating}}</p>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        rating: {
            type: Number,
            required: true,
            default: 0
        },
        maxStars: {
            type: Number,
            default: 5
        }
    }
};
</script>

<style scoped>
</style>
