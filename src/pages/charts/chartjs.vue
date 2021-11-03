<template>
  <div>
      <Breadcrumbs main="" title="Statistics"/>
        <div class="container-fluid">
          <div class="row">
            <div class="col-xl-6 col-md-12">
              <px-card title="Bar Chart">
                <div slot="headerAction" ></div>
                <div slot="with-padding" >
                  <Bar class="chart-height" :width="200" :height="200"/>
                </div>
              </px-card>
            </div>
            <div class="col-xl-6 col-md-12">
              <px-card title="Line Graph">
                <div slot="headerAction" ></div>
                <div slot="with-padding" >
                  <Lines class="chart-height" :height="200"/>
                </div>
              </px-card>
            </div>

            <div class="col-xl-6 col-md-12">
              <px-card title="Line Chart">
                <div slot="headerAction" ></div>
                <div slot="with-padding" >
                  <Linechart class="chart-height" :height="200"/>
                </div>
              </px-card>
            </div>

            <div class="col-xl-6 col-md-12">
              <px-card title="Doughnut Chart">
                <div slot="headerAction" ></div>
                <div slot="with-padding" >
                  <Doughnut class="chart-height" :height="200"/>
                </div>
              </px-card>
            </div>

          </div>
        </div>
        <!-- Container-fluid Ends-->
  </div>
</template>
<script>
  import Lines from './chartjs/Line';
  import Bar from './chartjs/Bar';
  import Linechart from './chartjs/Linechart';
  import Doughnut from './chartjs/Doughnut';
  
  export default {
    components:{ Bar, Lines,Linechart,Doughnut }  
  };
</script>

<style>
</style>