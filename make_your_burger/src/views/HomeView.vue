<template>
  <div>
    <Banner/>
    <div class="main-container">
      <BurgerForm />
    </div>
  </div>
</template>

<script >
import Banner from "@/components/Banner.vue";
import BurgerForm from "@/components/BurgerForm.vue";

export default {
  name: "HomeView",
  components: {
    Banner, 
    BurgerForm
  },
}
</script>
