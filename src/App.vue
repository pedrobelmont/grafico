<template>
  <div id="app">
    <GraficoPareto :data="data" :mes="mes" />
  </div>
</template>
<script>
import GraficoPareto from "@/components/grafico1.vue";
import { ref } from "vue";
export default {
  name: "App",
  components: { GraficoPareto },
  setup() {
    const mes = ref(["Jan", "Fev", "Mar", "Abr", "Mai"]);
    const data = ref([600, 550, 750, 250, 700]);
    console.log(data.value);
    return {
      data,
      mes,
    };
  },
};
</script>