<template>
    <div class="flex items-center justify-center h-screen">
        <form @submit.prevent="sendForm" class="
            border-8 border-gray-200 rounded-xl
            w-[500px] -mt-14 p-8
            flex flex-col gap-y-3
        ">
            <label class="flex flex-col gap-y-0.5 text-gray-500 focus-within:text-brand-2">
                <span class="text-lg">Заголовок</span>
                <input class="
                px-3 py-2 border-2 border-gray-300 rounded-md shadow-inner
                focus-visible:outline-brand-2 outline-2
            " type="text" v-model="data.title">
            </label>

            <label class="flex flex-col gap-y-0.5 text-gray-500 focus-within:text-brand-2">
                <span class="text-lg">Какой сервис</span>
                <select class="px-3 py-2 border-2 border-gray-300 rounded-md shadow-inner
                    focus-visible:outline-brand-2 outline-2"
                        v-model="data.service">
                    <option class="" value="shop">Магазин</option>
                    <option value="delivery">Доставка</option>
                    <option value="app">Приложение</option>
                </select>
            </label>

            <input v-model="data.datetime" type="datetime-local" />

            <label class="flex flex-col gap-y-0.5 text-gray-500 focus-within:text-brand-2">
                <span class="text-lg">Описание</span>
                <textarea class="
                    px-3 py-2 border-2 border-gray-300 rounded-md shadow-inner
                    focus-visible:outline-brand-2 outline-2
                " rows="6" v-model="data.description" />
            </label>

            <label class="flex flex-col gap-y-0.5 text-gray-500 focus-within:text-brand-2">
                <span class="text-lg">Рейтинг</span>
                <div class="flex items-center">
                    <label v-for="star in 5" :key="star" class="flex items-center cursor-pointer">
                        <input type="radio" :id="'star' + star" :value="star" v-model="data.rating" class="hidden" />
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" :class="{ 'text-yellow-400 fill-current': star <= data.rating }" viewBox="0 0 24 24" @click="data.rating = star">
                            <path d="M12 1l3.09 6.3L22 9.27l-5.54 5.4L18.72 23 12 19.4 5.28 23l1.26-8.33L2 9.27l6.91-1.97z" />
                        </svg>
                    </label>
                    <p class="ms-1 text-sm font-medium text-gray-500 dark:text-gray-400">{{data.rating}}</p>
                </div>
            </label>
            <button class="
                grid place-content-center w-full p-2 mt-1 border-2 border-gray-300 rounded-md shadow-sm outline-none bg-white
                text-lg font-semibold tracking-wide text-gray-400
                transition-all
                focus-visible:bg-brand-2 focus-visible:border-green-700 focus-visible:text-green-800 focus-visible:shadow-xl
                hover:bg-brand-2 hover:border-green-700 hover:text-green-800 hover:shadow-xl
            " type="submit">Отправить</button>
        </form>
    </div>
</template>

<script setup lang="ts">
import { reactive } from 'vue';
import axios from 'axios';
import env from '@/env.json'
import router from '@/router';

const data = reactive({
    description: '',
    title: '',
    service: '',
    datetime: '',
    rating: 0,
});

const sendForm = async () => {
    try {
        const response = await sendFormImpl();
        if (!response.data.id)
            throw new Error('Ошибка сервера');
        await router.push({ name: 'feedback-show', params: { id: response.data.id } });
    } catch (error) {
        alert(error);
    }
}

const sendFormImpl = async () => {
    return await axios.post<StoreFeedbackResponse>(env.backend_url + '/feedbacks', {
        'description': data.description,
        'title': data.title,
        'service': data.service,
        'datetime':  new Date(data.datetime).getTime(),
        'rating':  data.rating
    });
}

interface StoreFeedbackResponse {
    id: number
}
</script>
