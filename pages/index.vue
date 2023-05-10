<template>
 <div class="flex flex-col items-center justify-center bg-gray-600 min-h-screen">
   <h1 class="text-center mb-4 text-4xl font-extrabold leading-none tracking-tight text-gray-900 md:text-5xl lg:text-6xl dark:text-white">
   PUISSANCE 4 ULTIME <br> EN NUXTJS</h1>
   <br>
   <div class="flex-col" >
     <div class="text-center mb-4">
        <h3 class="text-3xl font-extrabold leading-none tracking-tight dark:text-white">
          Tu veux jouer contre un <span class="text-transparent bg-clip-text bg-gradient-to-r to-emerald-600 from-sky-400">robot</span> ?
        </h3>
       <br>

       <nuxt-link to="/robot" class="w-full mb-10 text-white bg-gradient-to-br from-green-400 to-blue-600 hover:bg-gradient-to-bl focus:ring-4 focus:outline-none focus:ring-green-200 dark:focus:ring-green-800 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2">Clique ici</nuxt-link>
     </div>



     <div class="text-center mb-4">
        <h3 class="text-3xl font-extrabold leading-none tracking-tight dark:text-white">
          Tu veux jouer contre ton <span class="text-transparent bg-clip-text bg-gradient-to-r from-red-200 via-red-300 to-yellow-200 hover:bg-gradient-to-bl focus:ring-4 focus:outline-none focus:ring-red-100"> pote </span> ?
        </h3>
       <br>

        <nuxt-link class="w-full mb-10 text-gray-900 bg-gradient-to-r from-red-200 via-red-300 to-yellow-200 hover:bg-gradient-to-bl focus:ring-4 focus:outline-none focus:ring-red-100 dark:focus:ring-red-400 font-medium rounded-lg text-sm px-5 py-2.5 text-center mr-2 mb-2" to="/normal">Clique ici</nuxt-link>

     </div>


   </div>

 </div>
</template>

<script lang="ts">
import Vue from 'vue'

export default Vue.extend({
  name: 'PuissancePage',
})
</script>
