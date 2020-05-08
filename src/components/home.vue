<template>
  <div class="home">
    <div class="breadcrumbs">

    </div>
    <div class="filter-controls">
      <div class="filter-select">
        <select class="form-control filter-inputs" v-model="filterDept">
          <option value="">Department</option>
          <option v-for="dept in currentDepts">{{dept}}</option>
        </select>
        <button class="btn btn-info clear-search" v-on:click="clearSearch()">Clear</button>
      </div>
      <input class="form-control filter-inputs" type="text" v-model="search" placeholder="Search" />
    </div>
    <div class="pages">
      <div>
        <div>
          <h5 class="title">Employee Directory</h5>
        </div>
      </div>
    </div> 
    <div class="jobs">
      <div class="job" v-for='person in filteredPeople'>
        <router-link :to="{ name: 'profile', params: { id: person.id }}">
          <p class="title"><img class="employee-thumb" alt="" :src="person.headshot.thumbnail" /> <strong>{{person.name.first}} {{person.name.last}}</strong>, {{person.job_title}} | {{person.department}}</p> 
        </router-link>
      </div>
    </div> 
  </div>
</template>
<script>
export default {
  name: 'home',
  data () {
    return {
      filterDept: '',
      filterTitle: '',
      search: '',
      employeeData: []
    }
  },
  methods: {
    clearSearch: function(){
      this.search = '';
      this.filterDept = '';
    }
  },
  computed: {
    filteredPeople: function() {
      return this.employeeData.filter((person) => {
        let fullname = person['name']['first'] + ' ' + person['name']['last'];
        return (fullname.toLowerCase().match(this.search.toLowerCase()) &&  person.department.toLowerCase().match(this.filterDept.toLowerCase()));
      }); 
    },
    currentDepts: function(){
      const depts = [];
      this.filteredPeople.forEach(el => depts.push(el.department));
      let uniqueDepts = [...new Set(depts)];
      return uniqueDepts;
    }
  },
  created: function() {
    var _this = this;
    const url = 'https://codepen.io/cardinalfinancial/pen/XQKXby.js';
    let xhr = new XMLHttpRequest();
    xhr.open('GET', url);
    xhr.responseType = 'json';
    xhr.send();
    xhr.onload = function() {
      _this.employeeData = xhr.response['results'];
    }
  }
}
</script>
<style scoped>
.home {margin-bottom: 2rem;}

.employee-thumb {margin-right: 1rem;}

.filter-controls {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}

.filter-select {
  display: flex;
}

select.filter-inputs {
  width: auto;
  padding: 6px 12px;
  color: #58585b;
  outline: 0;
  font-size: 14px;
  border-radius: 0;
  background-color: white;
  border: 1px solid #58585b;
}

.clear-search {
  margin-left: .5rem;
  padding: 6px 12px;
}

input.filter-inputs {
  width: auto;
  padding: 6px 12px;
  color: #58585b;
  outline: 0;
  font-size: 14px;
  border-radius: 0;
  background-color: white;
  border: 0;
  border: 1px solid #58585b;
}

body.page-template-template-careers-search #careersVue .right-rail .jobs .job .title{ padding: 1em .75em; }

</style>