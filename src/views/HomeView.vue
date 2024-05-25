<template>
  <BannerComponent/>
  <div class="main-container">
    

   <h1>Monte o seu burger</h1>
   <BurgerFormComponentVue/>
  </div>
</template>

<script>
import BannerComponent from '@/components/BannerComponent.vue'

import BurgerFormComponentVue from '@/components/BurgerFormComponent.vue'
// @ is an alias to /src


export default {
  name: 'HomeView',
  components: {
   BannerComponent,
   BurgerFormComponentVue
  }
}
</script>
