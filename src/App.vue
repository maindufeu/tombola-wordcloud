<template>
  <div>
    <div>
    <img src="https://github.com/AI-ML-Lab/resources/blob/main/images/m_dimensiones.gif?raw=true" class="logo" alt="Dimensiones" />
    <h1>Minsait</h1>
  </div>
    <WordCloud :text="sampleText" />
  </div>
  <div>
    <MatterAnimation />
  </div>
</template>

<script>
import WordCloud from "@/components/WordCloud.vue";
import MatterAnimation from '@/components/MatterAnimation.vue';

export default {
  components: {
    WordCloud,
    MatterAnimation
  },
  data() {
    return {
      sampleText: "Aquí va el texto que quieras convertir en una nube de palabras."
    };
  }
};
</script>
