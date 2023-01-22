<template>
 <div class="flex flex-col">
  <div class="overflow-x-auto">
   <div class="p-1.5 w-full inline-block align-middle">
    
    <div class="overflow-hidden border rounded-lg">
     <table class="min-w-full divide-y divide-gray-200">
      <thead class="bg-gray-50">
       <tr>
        <th scope="col" class="px-6 py-3 text-xs font-bold text-left text-gray-500 uppercase">
         ID
        </th>
        <th scope="col" class="px-6 py-3 text-xs font-bold text-left text-gray-500 uppercase">
         Name
        </th>
        <th scope="col" class="px-6 py-3 text-xs font-bold text-left text-gray-500 uppercase">
         Url
        </th>
        <th scope="col" class="px-6 py-3 text-xs font-bold text-left text-gray-500 uppercase">
         Sprites
        </th>
        <th scope="col" class="px-6 py-3 text-xs font-bold text-right text-gray-500 uppercase">
         Edit
        </th>
        <th scope="col" class="px-6 py-3 text-xs font-bold text-right text-gray-500 uppercase">
         Delete
        </th>
       </tr>
      </thead>
      <tbody class="divide-y divide-gray-200">
       <tr v-for="poko in pokemon.data" :key="poko.id">
       {{ poko.current_page }}
        <td  class="px-6 py-4 text-sm font-medium text-gray-800 whitespace-nowrap">
         {{ poko.id }}
        </td>
        <td  class="px-6 py-4 text-sm font-medium text-gray-800 whitespace-nowrap">
         {{ poko.name }}
        </td>
        <td class="px-6 py-4 text-sm font-medium text-gray-800 whitespace-nowrap">
         {{ poko.url }}
        </td>
        <td class="px-6 py-4 text-sm font-medium text-gray-800 whitespace-nowrap">
         <img :src= poko.sprites[0].back_default> 
        </td>
        <td class="px-6 py-4 text-sm font-medium text-right whitespace-nowrap">
         <button class="btn btn-sm btn-danger" @click="updateData(poko.id,poko.name) "> Edit</button>
        </td>
        
        <td class="px-6 py-4 text-sm font-medium text-right whitespace-nowrap">
         <button class="btn btn-sm btn-danger" @click="deleteData(poko.id) "> Delete</button>
        </td>
        

       </tr>
        
      </tbody>
     </table>
      <TailwindPagination :data="pokemon" @pagination-change-page="getResults" />
    </div>
     
   </div>
    
  </div>
 </div>

</template>

<script setup>

import { ref } from 'vue';
import { TailwindPagination } from 'laravel-vue-pagination';



 
const pokemon = ref({});

const getResults = async (page = 1) => {
 const response = await fetch("http://127.0.0.1:8000/api/pokemon");
 pokemon.value = await response.json();
}

getResults();

  async function deleteData(id){
  const {data:res} = await useFetch("http://127.0.0.1:8000/api/pokemon/"+id, { method: "DELETE" });
  const message = res.value;
  alert(message.message);
   window.location.href = '/';
  
}
 async function updateData(id,url){
  let newName= prompt("Please Enter New Name:", name);
  const request= {
   method: "PUT",
   headers: { "Content-Type": "application/json" },
   body: { newName: newName }
  };
  const { data: res } = await useFetch("http://127.0.0.1:8000/api/pokemon/" + id, request);
  const message = res.value;
  alert(message.message);
  window.location.href = '/';
 

  
  
 }

</script>