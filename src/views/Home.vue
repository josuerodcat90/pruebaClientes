<template>
  <div>
    <h1>LISTA DE CLIENTES</h1>
    <div>
      <CumstomerList />
    </div>
  </div>
</template>
<script>
// @ is an alias to /src
import CumstomerList from "@/components/CumstomerList.vue";

export default {
  name: "Home",
  components: {
    CumstomerList,
  },
};
</script>
