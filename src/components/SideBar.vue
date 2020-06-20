<!--template>
  <div>Sidebar</div>
</template-->
<template>
  <div>
    <b-button v-b-toggle.sidebar-1>Toggle Sidebar</b-button>
    <b-sidebar id="sidebar-1" :visible="true" title="Sidebar" shadow>
      <div>
        <b-form v-if="show">
          <b-form-group id="input-group-3" label="Project Ownership:" label-for="input-3">
            <b-form-select
              id="input-3"
              v-model="form.ownership"
              :options="ownerships"
              required
              @change="onChangeOwnerShip(form.ownership)">
            </b-form-select>
          </b-form-group>
          <b-form-group id="input-group-3" label="Project Status:" label-for="input-3">
            <b-form-select
              id="input-3"
              v-model="form.status"
              :options="statuses"
              required
              @change="onChangeStatus(form.status)">
            </b-form-select>
          </b-form-group>
      </b-form>
    </div>
    <div>
    </div>
    <b-list-group>
        <b-list-group-item v-for="feature of filteredData" :key="feature.id" :lng="feature.geometry.coordinates[0]" :lat="feature.geometry.coordinates[1]" @click="onClickGetCoordinates(feature)"> Project Title:  {{feature.properties.project.Title}}</b-list-group-item>        
    </b-list-group>
    </b-sidebar>
  </div>
</template>
<script>
export default {
  name: 'SideBar',
  props: {
    msg: String,
    mapJson: [Object, Array]
  },
  data() {
      return {
        data: [],
        filteredData: [],
        form: {
          ownership: null,
          status: null
        },
        ownerships:  [],
        statuses:  [],
        show: true
      }
    },
    methods: {
      onChangeOwnerShip(own){
        this.filteredData = [];
        if (this.form.status == null){
          for(var i = 0; i < this.mapJson.features.length; i++){
            if(this.mapJson.features[i].properties.project.Ownership == own){
              this.filteredData.push(this.mapJson.features[i])
            }
          }
        }else{
          for(var j = 0; j < this.mapJson.features.length; j++){
            if(this.mapJson.features[j].properties.project.Status == this.form.status && this.mapJson.features[j].properties.project.Ownership == own){
              this.filteredData.push(this.mapJson.features[j])
            }
          }
        }
      },
      onChangeStatus(status){
        this.filteredData = [];
        if(this.form.ownership == null){
          for(var i = 0; i < this.mapJson.features.length; i++){
            if(this.mapJson.features[i].properties.project.Status == status){
              this.filteredData.push(this.mapJson.features[i])
            }
          }
        }else{
          for(var j = 0; j < this.mapJson.features.length; j++){
            if(this.mapJson.features[j].properties.project.Status == status && this.mapJson.features[j].properties.project.Ownership == this.form.ownership){
              this.filteredData.push(this.mapJson.features[j])
            }
          }
        }
      },
      onClickGetCoordinates(feature){
        this.$emit('filteredData', feature);
      }
    },
    
    created(){
        try{
        // axios.get(baseURL).then((response) => {
          // this.mapJson;
          this.filteredData = this.mapJson.features;
          for(var i = 0; i < this.mapJson.features.length; i++){
            if(!this.ownerships.includes(this.mapJson.features[i].properties.project.Ownership)){
              this.ownerships.push(this.mapJson.features[i].properties.project.Ownership);
            }
            if(!this.statuses.includes(this.mapJson.features[i].properties.project.Status)){
              this.statuses.push(this.mapJson.features[i].properties.project.Status);
            }
          }            
        // })
        }catch(e){
        console.error(e)
        }
    }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
