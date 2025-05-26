<template>
    <div>
        <UBreadcrumb :items="[
            { label: 'Home', to: '/' },
            { label: 'Profile', to: '/profile' }
        ]" class="mb-4" />
        <div class="flex gap-3 mb-2">
            <div
                class="w-8 h-8 p-1 bg-gray-200 rounded flex items-center justify-center hover:bg-gray-300 transition-colors cursor-pointer">
                <img src="@/assets/image/printer-svgrepo-com.svg" alt="Printer" class="w-4 h-4" />
            </div>
            <div
                class="w-8 h-8 p-1 bg-gray-200 rounded flex items-center justify-center hover:bg-gray-300 transition-colors cursor-pointer">
                <p class="text-black text-xs font-bold leading-none">PDF</p>
            </div>
            <div
                class="w-8 h-8 p-1 bg-gray-200 rounded flex items-center justify-center hover:bg-gray-300 transition-colors cursor-pointer">
                <p class="text-black text-xs font-bold leading-none">DOC</p>
            </div>
            <div
                class="w-8 h-8 p-1 bg-gray-200 rounded flex items-center justify-center hover:bg-gray-300 transition-colors cursor-pointer">
                <img src="@/assets/image/resume-svgrepo-com.svg" alt="Resume" class="w-4 h-4" />
            </div>
            <div
                class="w-8 h-8 p-1 bg-gray-200 rounded flex items-center justify-center hover:bg-gray-300 transition-colors cursor-pointer">
                <img src="@/assets/image/icons8-trash.svg" alt="Delete" class="w-4 h-4" />
            </div>
            <div
                class="w-8 h-8 p-1 bg-gray-200 rounded flex items-center justify-center hover:bg-gray-300 transition-colors cursor-pointer">
                <img src="@/assets/image/icons8-submit-resume-50.png" alt="Submit" class="w-4 h-4" />
            </div>
            <div
                class="w-8 h-8 p-1 bg-gray-200 rounded flex items-center justify-center hover:bg-gray-300 transition-colors cursor-pointer">
                <img src="@/assets/image/favorite-svgrepo-com.svg" alt="Favorite" class="w-4 h-4" />
            </div>
        </div>
        <div class="flex gap-2">
            <div>
                <img src="@/assets/image/no_avatar_2048x2048_3.png" alt="Avatar"
                    class="w-40 h-40 object-cover object-center" />

            </div>
            <div>
                <p>{{ !data ? 'Загрузка...' : !data.name ? 'Имя не указано' : data.name }}</p>
                <p>
                    {{ !data ? 'Загрузка...' : !data.status ? 'Нет информации' : data.status ? 'Просмотрено' :
                        'Непросмотрено' }}
                </p>
                <p>
                    {{ !data ? 'Загрузка...' : !data.age ? 'Возраст не указан' : `Возраст:${data.age}` }}
                </p>
                <div class="flex items-center">
                    <img src="@/assets/image/phone-call.png" alt="phone" class="w-5 h-5 mr-2" />
                    <p>{{ !data ? 'Загрузка...' : !data.phone ? 'Номер не указан' : data.phone }}</p>
                </div>
                <div class="flex items-center">
                    <img src="@/assets/image/icons8-email-50.png" alt="email" class="w-5 h-5 mr-2" />
                    <p>{{ !data ? 'Загрузка...' : !data.email ? 'Email не указан' : data.email }}</p>
                </div>
            </div>
        </div>
        <p class="font-medium mt-4 mb-4">Дела:</p>
        <div class="flex flex-wrap gap-3 justify-start">
            <div v-for="(block, index) in affairsBlock" :key="index"
                class="w-[100px] h-[50px] flex items-center justify-center border-2 cursor-pointer" :class="{
                    'border-green-800': block.completed,
                    'bg-green-600': block.completed,
                    'border-green-100': !block.completed,
                    'bg-transparent': !block.completed,
                }" @click="toggleStatusForAffairs(index)">
                <p class="text-xs font-medium text-center px-1 text-white">
                    {{ block.text }}
                </p>
            </div>
        </div>
        <p class="font-medium mt-4 mb-4">Статус рассмотрения:</p>
        <div class="max-w-screen-lg flex flex-wrap gap-3 justify-start">
            <div v-for="(block, index) in statusBlock" :key="index"
                class="status-block relative w-[150px] h-[35px] flex items-center justify-center border-2 cursor-pointer"
                :class="{
                    '!border-slate-500': block.completed,
                    'bg-slate-500': block.completed,
                    '!border-slate-50': !block.completed,
                    'bg-slate-50': !block.completed,
                }" @click="toggleStatusForStatus(index)">
                <p class="text-xs font-medium text-center px-1"
                    :class="{ 'text-white': block.completed, 'text-black': !block.completed }">
                    {{ block.text }}
                </p>
            </div>
        </div>
    </div>
    <div class="mt-4">
        <p class="text-2xl mb-4">Pikabu отклик</p>
        <p class="text-1xl mb-4">Отклик с портала Pikabu</p>
        <div v-if="data && data.description" class="rounded cursor-pointer"
            @click="isDescriptionExpanded = !isDescriptionExpanded">
            <p v-if="isDescriptionExpanded">{{ data.description }}</p>
            <p v-else>{{ truncateText(data.description, 300) }}</p>
            <p class="text-sm text-gray-500 mt-1 mb-4">
                {{ isDescriptionExpanded ? 'Свернуть' : 'Развернуть' }}
            </p>
        </div>
        <p v-else-if="!data">Загрузка...</p>
        <p v-else-if="!data.description">Описание не указано</p>
    </div>
    <div>
        <p class="text-1xl mb-4">Сопроводительное письмо</p>
        <div class="bg-teal-600 p-5 flex">
            <div>
                <img src="@/assets/image/icons8-info-50.png" alt="info" class="w-10 h-10 mr-3" />
            </div>
            <div>
                <p class="font-normal">Файл портфолио:</p>
                <p class="text-blue-700 font-semibold">Резюме:</p>
            </div>
        </div>
    </div>
</template>

<script lang="ts" setup>
import { ref, onMounted } from "vue";

interface ApiResponse {
    id: number;
    name: string;
    status?: boolean;
    age?: number;
    phone?: string;
    email?: string;
}

interface Block {
    text: string;
    completed: boolean;
}

const affairsBlock = ref<Block[]>([
    { text: "Собеседование запланировано", completed: true },
    { text: "создать видеозвонок", completed: false },
    { text: "Запланировать событие", completed: false },
    { text: "отправить запрос", completed: false },
]);

const statusBlock = ref<Block[]>([
    { text: "Новое", completed: true },
    { text: "Просмотрено", completed: false },
    { text: "Отправилено приглашение", completed: false },
    { text: "Назначено собеседование", completed: false },
    { text: "Не дошел", completed: false },
    { text: "Проведено собеседование", completed: false },
    { text: "Ожидание ответа соискателя", completed: false },
    { text: "Принятие решения", completed: false },
    { text: "Принят", completed: false },
    { text: "Отклонено/Отказ", completed: false },
    { text: "Архивировано", completed: false },
]);

function toggleStatusForAffairs(index: number) {
    affairsBlock.value[index].completed = !affairsBlock.value[index].completed;
}
function toggleStatusForStatus(index: number) {
    statusBlock.value[index].completed = !statusBlock.value[index].completed;
}

const data = ref<ApiResponse | null>(null);
const error = ref<string | null>(null);

onMounted(async () => {
    try {
        const response = await fetch("https://dev.jobcart.ru/wp-json/test/v2/app");
        if (!response.ok) {
            throw new Error(`HTTP ошибка! Статус: ${response.status}`);
        }
        const jsonData: ApiResponse = await response.json();
        data.value = jsonData;
        console.log(data.value);
    } catch (err) {
        error.value = (err as Error).message;
    }
});

const isDescriptionExpanded = ref(false);

function truncateText(text: string, maxLength: number): string {
    if (!text) return '';
    if (text.length <= maxLength) return text;
    return text.slice(0, maxLength) + "...";
}

</script>

<style scoped>
.status-block {
    position: relative;
}

.status-block::after {
    content: "";
    position: absolute;
    right: -11px;
    top: 50%;
    transform: translateY(-50%);
    width: 0;
    height: 0;
    border-top: 18px solid transparent;
    border-bottom: 18px solid transparent;
    border-left: 10px solid #f8fafc;
}

.status-block.bg-slate-500::after {
    border-left-color: #64748b;
}
</style>
