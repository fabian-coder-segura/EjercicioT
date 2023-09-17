<script setup lang="ts">

import{ref} from 'vue';
import axios from 'axios';
import { type } from 'os';


interface Producto{
    category: string;
    descripcion: string;
    id:number;
    image:string;
    price:number;
    rating: {
        rate:number;
        count:number;
    };
    title: string;
};

type PruebaType = Producto | string | null;

const data = ref<PruebaType>();
data.value = 'Test';
data.value = null;



const products = ref<Array<any>>([]);

axios.get('https://fakestoreapi.com/products',{
    headers: {
        Authorization: '1234567',
        'keyAuth': 'test12345'
    }
})
.then(response=>{
    console.log(response)
    products.value = response.data;
})
.catch(error=>{
    console.log(error);
    alert('Error al consumir el servicio de productos');
})
.finally(()=>{
    console.log('Se finalizo la consulta del servicio');
});


</script>
<template>
    <div v-for="item of products">
        <img :src="item.image" alt="Producto">
    </div>
<h1></h1>
</template>
<style>

</style>