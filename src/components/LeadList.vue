<script setup>
import axios from 'axios';
import { onMounted, ref } from 'vue';
import LeadItem from './LeadItem.vue';
import ModalWindow from './ModalWindow.vue';

// .env не создавал, так как не было необходиммости
let token = ref('eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsImp0aSI6IjhjYzdjOTlhYTc4NjAyZmMyMGJjOGNmNzZmZGJkMWY5Y2M5MDAzYzdlNjc3ZTU1ZTc1MzgzOGM1NWQ3MDM4ZDRjZmFlYzA2NmVmZmU3ZDM3In0.eyJhdWQiOiIyOTg3ODU1Mi0yNDYwLTRhNjMtOWM1OS1hODI0OTVlOTE0ZjEiLCJqdGkiOiI4Y2M3Yzk5YWE3ODYwMmZjMjBiYzhjZjc2ZmRiZDFmOWNjOTAwM2M3ZTY3N2U1NWU3NTM4MzhjNTVkNzAzOGQ0Y2ZhZWMwNjZlZmZlN2QzNyIsImlhdCI6MTcyNzc4ODU2MSwibmJmIjoxNzI3Nzg4NTYxLCJleHAiOjE3NTQ3ODQwMDAsInN1YiI6IjExNTg1NDQyIiwiZ3JhbnRfdHlwZSI6IiIsImFjY291bnRfaWQiOjMxOTgwNzc0LCJiYXNlX2RvbWFpbiI6ImFtb2NybS5ydSIsInZlcnNpb24iOjIsInNjb3BlcyI6WyJjcm0iLCJmaWxlcyIsImZpbGVzX2RlbGV0ZSIsIm5vdGlmaWNhdGlvbnMiLCJwdXNoX25vdGlmaWNhdGlvbnMiXSwiaGFzaF91dWlkIjoiZjdhZWY3ZWUtNTYxYS00NmQwLWFjMjUtNGY5NDI2NjcxMjk0IiwiYXBpX2RvbWFpbiI6ImFwaS1iLmFtb2NybS5ydSJ9.YOd-yJcm4CCsEbme7i06iL9eaEETD51RVhxfdW7VBePSGH-XNg06o9t-icDVOi1nnjqP4k9KK0w8oAUF5zDfk73CXXLFQYvCS40ggnXqM5EgHIY7mjBmKne5hMlI86fvpJtJn_gbdk1ab1PrwLvUQ7agqw1OagLJb6biBi4p7XNyxiXspqB4wfNxDf4FPuC8d-ter-Zg1nw0T9KPUSLAXQqUpnXn5bxWQKEDRgXvhJ0jZccy4aj77Utz5lW_ZSL2TPUeH3vIXtwfiPBnUyAurVYel_561-qAPBKKO6nh8OES1rXrk9XZwfz6Jmh32-hRyUPCQnSpH-APuQH_BZslkw')

const config = {
    headers: {
        'Authorization': `Bearer ${token.value}`,
        'Content-Type': 'application/json'
    }
};

const url = ref('https://oatoffprofile.amocrm.ru/api/v4/leads');

let result = ref([]);
let currentIndex = 0;

async function getLeads() {
    try {
        const response = await axios.get(url.value, config);
        const leads = response.data._embedded.leads;

        const leadsToFetch = leads.slice(currentIndex, currentIndex + 3);
        leadsToFetch.forEach(item => {
            result.value.push({
                id: item.id,
                name: item.name,
                price: item.price,
                created_at: item.created_at,
                closest_task_at: item.closest_task_at,
            });
        });

        currentIndex += 3;

        if (currentIndex < leads.length) {
            setTimeout(getLeads, 1000);
        }
    } catch (error) {
        console.error(error);
    }
}

const modal = ref(null);

const openModal = (item) => {
    modal.value.open(item);
};


onMounted(() => {
    getLeads();
})

</script>

<template>
    <div class="table-title">Сделки amoCRM</div> 
    <table class="table-fill"> 
        <thead> 
            <tr> 
                <th class="text-left">ID</th> 
                <th class="text-left">Название</th> 
                <th class="text-left">Бюджет</th> 
            </tr> 
        </thead> 
        <tbody class="table-hover">
            <LeadItem
                v-for="(item, index) in result"
                @click="openModal(item)"
                :key="index"
                :id="item.id"
                :name="item.name"
                :price="item.price"
            />
        </tbody> 
    </table> 
    <ModalWindow ref="modal"/>
</template>