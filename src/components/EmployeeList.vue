<template>
  <div>
    <h1>Employee List</h1>
    <table border="1px" style="width: 100%;">
      <tr>
        <td>Id</td>
        <td>Name</td>
        <td>Salary</td>
        <td>Age</td>
      </tr>
      <tr v-for="item in list" v-bind:key="item.id">
        <td>{{item.id}}</td>
        <td>{{item.employee_name}}</td>
        <td>{{item.employee_salary }}</td>
        <td>{{item.employee_age }}</td>
      </tr>
      <tr v-for="(entry, index) in paginatedEntries" :key="'entry-'+index">
        <td v-for="(col, ind) in columns" :key="'col-'+ind">
          <template v-if="col.type === 'slot'">
            <slot :entry="entry"></slot>
          </template>
          <div class="check" v-else-if="col.type === 'checkbox'">
            <input type="checkbox" class="checkInput" :ref="setCheckedRef" :value="entry[col.prop]" :checked="checks.includes(entry[col.prop])" @click="(checkedRefs[ind].checked === true) ? checks.push(entry[col.prop]) : removeChecked(entry[col.prop]); emit('checklist', checks)">
          </div>
          <template v-else>{{ entry[col.prop] }}</template>
        </td>
      </tr>
    </table>

  </div>

</template>
<script>
import Vue from 'vue';
import axios from 'axios'
import VueAxios from 'vue-axios'
Vue.use(VueAxios,axios)
export default{
  name:"EmployeeList",
  data(){
    return{list:undefined}
  },
  mounted()
  {
    Vue.axios.get('http://dummy.restapiexample.com/api/v1/employees')
    .then((resp)=>{
      this.list=resp.data.data;
      console.warn(resp.data.data)
    })
  }
}

</script>