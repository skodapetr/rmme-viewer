<template>
  <div style="height: 100%">
    <no-data v-if="!dataAvailable" />
    <d3-line-plot
      v-else
      :data="plotData"
      :text="plotText"
      :resize-notification="resizeNotification"
      :args="args"
    />
  </div>
</template>

<script>
  import LinePlot from "../d3js/line-plot";
  import NoData from "../ui/no-data";

  import {rangeByStep} from "./views-utils";
  import {STATUS_OK, STATUS_WARNING, STATUS_INVALID} from "../data-status";

  export default {
    "validator": validateData,
    "label": "GC content",
    //
    "name": "gc-content",
    "components": {
      "d3-line-plot": LinePlot,
      "no-data": NoData,
    },
    "props": {
      "data": {"type": Object, "required": true},
      "options": {"type": Object, "required": true},
      "resizeNotification": {"type": Object, "require": true},
    },
    "computed": {
      "dataAvailable": function(){
        return selectData(this.data) != null;
      },
      "plotData": function () {
        const data = selectData(this.data);
        const options = selectData(this.options);
        return [
          {
            "label": "First fragment",
            "color": options["gcf"],
            "y": data["gcf-y"],
            "x": data["gcf-x"]
          }, {
            "label": "Last fragment",
            "color": options["gcl"],
            "y": data["gcl-y"],
            "x": data["gcl-x"]
          }
        ];
      },
      "plotText": function() {
        const data = selectData(this.data);
        return data["text"];
      },
      "args": function() {
        const options = selectData(this.options);
        return {
          "margin": options["margin"],
        }
      }
    }
  };

  function selectData(data) {
    return data["gc-content"];
  }

  function validateData(data) {
    return STATUS_OK;
  }

</script>