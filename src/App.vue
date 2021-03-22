<template>
  <div id="app">
    <h1>D3 Visualization within Vue</h1>
    <svg width="800" height="600" id="viz"></svg>
  </div>
</template>

<script>
const d3 = require('d3');

export default {
  name: 'App',
  components: {

  },

  mounted(){
    let numbers = [100,250,160,80,200,300,120,240];

    // select visual enviroment: SvG
    const svg = d3.select('#viz');

    const scaleLength = d3.scaleLinear()
        .domain([0, d3.max(numbers)])
        .range([0, 600]);

    /*const scalePos = function(d,i){
      return 20 * i + 20;
    }*/

    const scalePos = d3.scaleBand()
        .domain(d3.range(numbers.length))
        .range([0,300])
        .round(true)
        .paddingInner(0.05) // distanza tra i vari rettangoli
        .paddingOuter(0.05);

    // ************ Creating the Bars ***********
    // join my data
    const rects = svg.selectAll('rect')
      .data(numbers)
      .join('rect');

    // update: join()
    rects
      .attr('x', 20)
      .attr('height', scalePos.bandwidth())
      .attr('y', (d,i) => scalePos(i))
      .attr('width', scaleLength)
      .attr('fill', '#ac0404');

    // ************* Creating the text labels *************
    const labels = svg.selectAll('text')
        .data(numbers)
        .join('text');

    labels
        .text((d) => d)
        .attr('x', scaleLength)
        .attr('y', (d,i) => scalePos(i))
        //.attr('dy', 10)
        .attr('dy', scalePos.bandwidth()/2)
        .attr('dx', -20)
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
