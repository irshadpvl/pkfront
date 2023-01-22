<template>
 <div class="bg-gray-100 min-h-screen text-gray-900">
  <Title>{{ title }}</Title>
  <nav class="bg-white shadow text-lg px-6 py-6">
   <div class="container mx-auto flex items-center justify-between px-6">
    <div>
     <NuxtLink to="/">Logo</NuxtLink>
    </div>
    <div>
     <ul class="flex space-x-12">
      <li>
       <NuxtLink to="/">Home</NuxtLink>
      </li>
      <li>
       <button @click="importAvatar">Import Pokemon</button>
       <span v-if="isLoading">Loading...</span>
      </li>
      
      
     </ul>
    </div>
   </div>
  </nav>
  <slot />
 </div>
</template>

<script setup>
const title = useState('title', () => 'Pokemon Avatar')
const isLoading = ref(false);

async function importAvatar(){
 isLoading.value=true;
 const { data: res } = await useFetch("http://127.0.0.1:8000/api/import");
 const message = res.value;
 console.log(message.message);
 alert("Itemhas been Loaded Please Refresh the Page to see Items"); 
 isLoading.value= message.message
 window.location.href = '/';

}
</script>

<style>
.router-link-active {
 font-weight: bold;
}
</style>