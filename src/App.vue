<template>
  <ejs-gantt ref="gantt"
             :dataSource="data"
             :taskFields="taskFields"
             :treeColumnIndex="1"
             :toolbar="toolbarOptions"
             :allowFiltering="true"
             :resources="resources"
             :resourceFields="resourceFields"
             v-on:toolbarClick="toolbarClick"
             :height="450"
             :rowHeight="60">
    <e-columns>
      <e-column field="TaskID" headerText="Task ID" width="120" textAlign="Right"></e-column>
      <e-column field="TaskName" textAlign="Left" width="200" :headerTemplate="'headerTemplate'"></e-column>
      <e-column field="resources" :template="'resourceTemplate'" :headerTemplate="'headerTemplate2'"></e-column>
      <e-column field="StartDate" headerText="Start Date" textAlign="Right" format="dd/MM/yyyy" width="120"></e-column>
      <e-column field="Duration" headerText="Duration" textAlign="Right" width="120"></e-column>
    </e-columns>
    <template v-slot:headerTemplate>
      <div>
        <img src="https://ej2.syncfusion.com/vue/demos/source/gantt/images/taskname.png" height="20" width="20"/> Task Name
      </div>
    </template>
    <template v-slot:headerTemplate2>
      <div>
        <img src="https://ej2.syncfusion.com/vue/demos/source/gantt/images/resources.png" height="20" width="20"/> Resources
      </div>
    </template>
    <template v-slot:resourceTemplate="{data}">
      <div v-if="data.ganttProperties.resourceNames">
        <img :src="'https://ej2.syncfusion.com/vue/demos/source/gantt/images/' + data.ganttProperties.resourceNames + '.png'"
              height="40" width="40"/>
      </div>
    </template>
  </ejs-gantt>
</template>

<script>
import {GanttComponent, ColumnsDirective, ColumnDirective, Toolbar, Filter} from '@syncfusion/ej2-vue-gantt'
import {projectNewData, editingResources} from './data.js'
export default {
  name: 'App',
  components: {
    'ejs-gantt': GanttComponent,
    'e-columns': ColumnsDirective,
    'e-column': ColumnDirective
  },
  provide:{
    gantt: [Toolbar, Filter]
  },
  methods:{
    toolbarClick(args){
      let ganttObj = this.$refs.gantt.ej2Instances;
      if(args.item.text === 'Quick Filter'){
        ganttObj.filterByColumn('TaskName', 'startswith', 'Research');
      } else{
        ganttObj.clearFiltering();
      }
    }
  },
  data(){
    return{
      data: projectNewData,
      resources:editingResources,
      toolbarOptions:[{text: 'Quick Filter', tooltipText: 'Quick Filter', id: 'Quick Filter' },
      { text: 'Clear Filter', tooltipText: 'Clear Filter', id: 'Clear Filter' }],
      resourceFields:{
        id:'resourceId',
        name:'resourceName'
      },
      taskFields:{
        id: 'TaskID',
        name: 'TaskName',
        startDate: 'StartDate',
        endDate: 'EndDate',
        duration: 'Duration',
        progress: 'Progress',
        dependency: 'Predecessor',
        child: 'subtasks',
        resourceInfo: 'resources'
      },
    }
  }
}
</script>

<style>
  @import url("https://cdn.syncfusion.com/ej2/material.css");
</style>
