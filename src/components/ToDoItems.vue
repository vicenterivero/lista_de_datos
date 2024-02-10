<script setup lang="ts">
import type { ITask } from './interface/ITask';
import { ref } from 'vue';

const props = defineProps<ITask>();
const check = ref(false)
const ItemClass = ref('item') 
const OrderCheck =() =>{
    ItemClass.value= 'item';
    if(check.value){
        ItemClass.value= 'item check';
    }
}

const emits = defineEmits(['item'])
const identify=(id:Number)=>{
    ItemClass.value = ItemClass.value.indexOf('delete')
    emits('item',id)
}
</script>

<template>
    <div :class="ItemClass">
        <input type="checkbox" v-model="check" @change="OrderCheck"/>
        <span>   {{ props.id  }}. {{props.name }}</span>
        
        <span @click="identify(props.id)">❌</span>
    </div>
</template>