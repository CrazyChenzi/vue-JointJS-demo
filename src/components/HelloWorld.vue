<template>
  <div class="hello">
    <div id="myholder"></div>
    <div id="myholder-small"></div>
    <g class="element basic Rect">
      <g class="rotatable">
          <g class="scalable">
              <rect />
          </g>
          <text />
      </g>
    </g>
    <path class="connection"/>
    <path class="marker-source"/>
    <path class="marker-target"/>
    <path class="connection-wrap"/>
    <g class="labels" />
    <g class="marker-vertices"/>
    <g class="marker-arrowheads"/>
    <g class="link-tools" />
  </div>
</template>

<script>
  const _ = require('lodash')
  const joint = require('jointjs')
  const backbone = require('../../static/js/backbone.js')
  import '../../static/css/joint.css'
  export default {
    name: 'HelloWorld',
    data() {
      return {}
    },
    mounted() {
      var graph = new joint.dia.Graph;
      var paper = new joint.dia.Paper({
        el: $('#myholder'),
        width: 1000,
        height: 1000,
        model: graph,
        gridSize: 1
      });
      var paperSmall = new joint.dia.Paper({
        el: $('#myholder-small'),
        width: 600,
        height: 100,
        model: graph,
        gridSize: 1
      });
      paperSmall.scale(.5);
      paperSmall.$el.css('pointer-events', 'none');

      var rect = new joint.shapes.basic.Rect({
        position: {
          x: 100,
          y: 30
        },
        size: {
          width: 100,
          height: 30
        },
        attrs: {
          rect: {
            fill: 'blue'
          },
          text: {
            text: 'my box',
            fill: 'white'
          }
        }
      });
      var rect2 = rect.clone();
      rect2.translate(300);
      rect.attr({
          rect: { fill: '#2C3E50', rx: 5, ry: 5, 'stroke-width': 2, stroke: 'black' },
          text: {
              text: 'my label', fill: '#3498DB',
              'font-size': 18, 'font-weight': 'bold', 'font-variant': 'small-caps', 'text-transform': 'capitalize'
          }
      });

      var link = new joint.dia.Link({
        source: {
          id: rect.id
        },
        target: {
          id: rect2.id
        }
      });
      link.attr({
          '.connection': { stroke: 'blue' },
          '.marker-source': { fill: 'red', d: 'M 10 0 L 0 5 L 10 10 z' },
          '.marker-target': { fill: 'yellow', d: 'M 10 0 L 0 5 L 10 10 z' }
      });
      link.set('vertices', [{ x: 300, y: 60 }, { x: 400, y: 60 }, { x: 400, y: 20 }])
      link.set('smooth', true)
      graph.addCells([rect, rect2, link]);
      //模型的事件
      graph.on('all', function(eventName, cell) {
        // console.log(arguments,"------------");
      });
      rect.on('change:position', function(element) {
        // console.log(element.id, ':', element.get('position'),);
      });
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1,
  h2 {
    font-weight: normal;
  }
  ul {
    list-style-type: none;
    padding: 0;
  }
  li {
    display: inline-block;
    margin: 0 10px;
  }
  a {
    color: #42b983;
  }
</style>
