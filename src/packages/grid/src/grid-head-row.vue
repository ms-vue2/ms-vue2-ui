<!--
Created by Bane.Shi.
Copyright MoenSun
User: Bane.Shi
Date: 2017/1/18
Time: 16:25-->
<template>
  <tr :ms-grid-head-row-id="msGridHeadRowId">
    <td v-for="(column,cellIndex) in columnRow"
        is="ms-grid-head-item"
        :column-row="columnRow"
        :column="column"
        :max-column-level="maxColumnLevel"
        :max-head-height="maxHeadHeight"
        :ms-grid-head-id="msGridHeadId"
        :allocated-height="allocatedHeight" >
    </td>
  </tr>
</template>
<script>
    import Vue from "vue";
    import PropsMixin from "./mixins/PropsMixin";
    import MethodsMixin from "./mixins/MethodsMixin";
    import LifecycleMixin from "./mixins/LifecycleMixin";

    import MsGridHeadItem from "./grid-head-item.vue";
    import BrowserType from "../../../utils/BrowserType.js";
    import bus from "./GridEvents";
    export default {
      name:'ms-grid-head-row',
      mixins:[PropsMixin,MethodsMixin,LifecycleMixin],
      props:{
        msGridHeadId:{
          type:String
        },
        headColumns:{
          type:Array
        },
        columnRow:{
          type:Array
        },
        headRowIndex:{
          type:Number
        },
        maxColumnLevel:{
          type:Number
        },
        maxHeadHeight:{
            type:Number,
            default(){
                return 0;
            }
        },
        allocatedHeight:{
            type:Number,
            default(){
                return 0;
            }
        }
      },
      data(){
        return {
          msGridHeadRowId:_.uniqueId("ms_grid_head_row_"),
        }
      },
      mounted(){
        let me = this;
        me.setRowHeight();
      },
      updated(){
        let me = this;
        
      },
      methods:{
        setRowHeight:function(){
            let me = this;
            let columnsRowCount = me.headColumns.length;
            let isLastRow = false;
            if(me.headRowIndex == columnsRowCount-1){
              isLastRow = true;
            }
            Vue.nextTick(function(){
          //    console.log(me.msGridHeadRowId+"|"+me.$el.clientHeight+"|"+me.$el.offsetHeight+"|"+$(me.$el).height());
              if(BrowserType.isIE() || BrowserType.isEdge()){
                 bus.$emit('ms-grid-head-row-ready',me.msGridId,me.msGridHeadId,isLastRow,$(me.$el).height());
              }else {
                  bus.$emit('ms-grid-head-row-ready',me.msGridId,me.msGridHeadId,isLastRow,me.$el.clientHeight);
              }

            });
        }
      },
      components: {
        MsGridHeadItem
      }
    }
</script>
<style>

</style>
