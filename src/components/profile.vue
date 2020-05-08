<template>
    <div class="profile">
        <div class="breadcrumbs">
            <router-link to="/">Employee Directory</router-link> // {{userInfo.name.first}} {{userInfo.name.last}}
        </div>
        <h1>{{userInfo.name.first}} {{userInfo.name.last}}</h1>
        <img class="employee-image" :src="userInfo.headshot.large" alt="" />
        <p>{{userInfo.job_title}}<br />{{userInfo.department}}</p>
        <h2>Contact</h2>
        <p>{{userInfo.phone}}<br />{{userInfo.email}}</p>
        <h3>Address</h3>
        <p>{{userInfo.location.street}}<br />{{userInfo.location.city}}, {{userInfo.location.state}} {{userInfo.location.postcode}}</p>
        <hr />
        <h2>Skills</h2>
        <ul>
            <li v-for='skill in userInfo.skills'>{{skill.id}}</li>
        </ul>
    </div>
</template>
<script>

 export default {
  name:'profile',
  data () {
   return {
    userId: this.$route.params.id,
    userInfo: []
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
    const employeeData = xhr.response['results'];
    employeeData.forEach(el => {
        if (el.id == _this.userId){
            _this.userInfo = el;
        }
    });
      
    }
  }
 }
</script>
<style scoped>
.employee-image {
    float: right;
}
 
</style>