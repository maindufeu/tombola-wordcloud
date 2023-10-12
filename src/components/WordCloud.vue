<!-- WordCloud.vue -->

<template>
  <div>
    <textarea v-model="inputText" @input="updateText" placeholder="Introduce tu texto aquí"></textarea>
    <div ref="wordCloud"></div>
  </div>
</template>

<script>
import * as d3 from "d3";
import cloud from "d3-cloud";

export default {
  name: "WordCloud",
  props: {
    initialText: {
      type: String,
      default: ""
    }
  },
  data() {
    return {
      words: [],
      inputText: this.initialText,
      colorScale: d3.scaleOrdinal(d3.schemeCategory10)  // Escala de colores
    };
  },
  watch: {
    inputText: "processText"
  },
  methods: {
    updateText() {
      this.processText();
    },
    processText() {
      let frequency = {};
      let wordsArray = this.inputText.split(/\s+/);
      
      wordsArray.forEach(word => {
        word = word.toLowerCase();
        frequency[word] = (frequency[word] || 0) + 1;
      });

      this.words = Object.keys(frequency).map(word => {
        return { text: word, size: frequency[word] * 10 };
      });

      this.renderWordCloud();
    },
    renderWordCloud() {
      const width = 400;
      const height = 400;
      
      const layout = cloud()
        .size([width, height])
        .words(this.words)
        .padding(5)
        .rotate(() => (~~(Math.random() * 6) - 3) * 30) 
        .fontSize(d => d.size)
        .on("end", words => {
          d3.select(this.$refs.wordCloud).html("");  // Limpia el contenedor antes de agregar una nueva nube
          d3.select(this.$refs.wordCloud)
            .append("svg")
            .attr("width", layout.size()[0])
            .attr("height", layout.size()[1])
            .append("g")
            .attr("transform", `translate(${layout.size()[0] / 2},${layout.size()[1] / 2})`)
            .selectAll("text")
            .data(words)
            .enter().append("text")
            .style("font-size", d => `${d.size}px`)
            .style("fill", d => this.colorScale(d.text))  // Asigna color
            .attr("text-anchor", "middle")
            .attr("transform", d => `translate(${[d.x, d.y]})rotate(${d.rotate})`)
            .text(d => d.text);
        });
        
      layout.start();
    }
  },
  mounted() {
    if (this.inputText) {
      this.processText();
    }
  }
};

</script>

<style scoped>
textarea {
  width: 100%;
  height: 100px;
  margin-bottom: 20px;
  padding: 10px;
}
</style>
