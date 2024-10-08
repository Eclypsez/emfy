<script setup>
import { ref, defineExpose, defineProps } from 'vue';
import CircleSvg from './CircleSvg.vue';

defineProps({
    item: {
      type: Object,
      default: () => ({})
    }
});

const isVisible = ref(false);
const itemLead = ref();

const open = (item) => {
    itemLead.value = item;
    isVisible.value = true;
    console.log(itemLead);
    
};

const close = () => {
    isVisible.value = false;
};

const formatUnixTimestamp = (timestamp) => {
    const date = new Date(timestamp * 1000);

    const day = String(date.getDate()).padStart(2, '0');
    const month = String(date.getMonth() + 1).padStart(2, '0');
    const year = date.getFullYear();

    return `${day}.${month}.${year}`;
}


defineExpose({ open, close });
</script>

<template>
    <div v-if="isVisible" class="modal-overlay" @click.self="close">
        <div class="modal">
            <p>Название: {{ itemLead.name }}</p>
            <p>Id: {{ itemLead.id }}</p>
            <p>Дата: {{ formatUnixTimestamp(itemLead.created_at) }}</p>
            <div class="flex">
                <div>Статус: </div>
                <CircleSvg :color="itemLead.closest_task_at"/>
            </div>
            <button class="button" @click="close">Закрыть</button>
        </div>
    </div>
</template>

<style scoped>
.modal-overlay {
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0, 0, 0, 0.5);
    display: flex;
    justify-content: center;
    align-items: center;
}

.modal {
    background: white;
    padding: 20px;
    border-radius: 5px;
}

.flex {
    display: flex;
    align-items: center;
}

.button {
    border: none;
    outline: none;
    padding: 4px 15px;
    background-color: rgb(0, 136, 255);
    color: white;
    font-size: 17px;
    border-radius: 5px;
    margin-top: 20px;
    cursor: pointer;
    width: 100%;
    transition: .2s;
}

.button:hover {
    background-color: rgb(0, 105, 197);
}
</style>