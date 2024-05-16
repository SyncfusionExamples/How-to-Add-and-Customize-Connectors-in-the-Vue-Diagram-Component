<template>
  <div class="container">
    <ejs-button class="button" @click="drawConnector">Draw Connector</ejs-button>
    <ejs-button class="button" @click="editConnector">Edit Connector</ejs-button>
    <ejs-button class="button" @click="removeConnector">Remove Connector</ejs-button>
  </div>
  <ejs-diagram ref="diagramObject" :width="width" :height="height" :nodes="nodes" :getNodeDefaults="getNodeDefaults"
    :connectors="connectors"></ejs-diagram>
</template>
<script>

let diagramInstance;

let ports = [{
  id: 'rightMidPort',
  offset: {
    x: 1,
    y: 0.5
  }
}];

let nodes = [
  {
    id: "startNode",
    offsetY: 80,
    shape: { type: "Flow", shape: "Terminator" },
    annotations: [
      {
        content: "Start"
      }
    ]
  },
  {
    id: "processNode",
    offsetY: 220,
    shape: { type: "Flow", shape: "Process" },
    annotations: [
      {
        content: "Process"
      }
    ],
    ports: ports
  },
  {
    id: "decisionNode",
    offsetY: 360,
    shape: { type: "Flow", shape: "Decision" },
    annotations: [
      {
        content: "Decision"
      }
    ],
    ports: ports
  },
  {
    id: "endNode",
    offsetY: 500,
    shape: { type: "Flow", shape: "Terminator" },
    annotations: [
      {
        content: "End"
      }
    ]
  }
];

let connectors = [
  {
    id: 'startToProcessConnector',
    sourceID: 'startNode',
    targetID: 'processNode',
    style: { strokeColor: 'Orange', strokeWidth: 5 },
    sourceDecorator: {
      shape: 'Diamond',
      style: { fill: 'Orange' }
    },
    targetDecorator: {
      shape: 'Diamond',
      style: { fill: 'Orange' }
    }
  },
  {
    id: 'decisionToProcessConnector',
    sourceID: 'decisionNode',
    targetID: 'processNode',
    type: 'Bezier',
    segments: [
      {
        type: "Bezier",
          point1: {
          x: 750,
          y: 350
        },
        point2: {
          x: 750,
          y: 250
        }
      }
    ],
    /*  type: "Orthogonal",
        segments: [
          {
            type: "Orthogonal", 
            direction: "Right", 
            length: 50
          }
        ], */
    targetPortID: 'rightMidPort',
    sourcePortID: 'rightMidPort',
    annotations: [{ content: 'No', style: { fill: 'white' } }]
  },
  {
    id: 'decisionToEndConnector',
    sourceID: 'decisionNode',
    targetID: 'endNode',
    annotations: [{ content: 'Yes', style: { fill: 'white' } }]
  }
];

import {
  DiagramComponent, DiagramTools
} from '@syncfusion/ej2-vue-diagrams';
import { ButtonComponent } from '@syncfusion/ej2-vue-buttons';

export default {
  components: {
    'ejs-diagram': DiagramComponent,
    'ejs-button': ButtonComponent
  },
  data: function () {
    return {
      width: '1102px',
      height: '602px',
      nodes: nodes,
      connectors: connectors,
      getNodeDefaults: (node) => {
        node.height = 80;
        node.width = 150;
        node.offsetX = 500;
      },
      drawConnector: () => {
        diagramInstance.tool = DiagramTools.DrawOnce;
        diagramInstance.drawingObject = { id: 'dynamicConnector', type: 'Polyline' }; //Straight //Freehand
      },
      editConnector: () => {
        diagramInstance.connectors[0].style = { strokeColor: 'Black', strokeWidth: 1 }
      },
      removeConnector: () => {
        diagramInstance.remove(diagramInstance.connectors[0]);
      }
    };
  },
  mounted: function () {
    diagramInstance = this.$refs.diagramObject.ej2Instances;
    diagramInstance.addConnector({
      id: 'processToDecisionConnector',
      sourceID: 'processNode',
      targetID: 'decisionNode'
    });
  }
};
</script>

<style>
@import "../node_modules/@syncfusion/ej2-vue-diagrams/styles/material.css";
@import "../node_modules/@syncfusion/ej2-vue-buttons/styles/material.css";

.container {
  text-align: left;
  margin-bottom: 20px;
}

.button {
  margin-right: 10px;
}
</style>