<template>
  <div id="app">
    <div id="tabledemo">
      <h3>Vue Datatable example</h3>Filter by anything:
      <input v-model="search">
      <hr>
      <DataTable :users="filteredUsers"/>
    </div>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import $ from "jquery";
import DataTable from "./components/DataTable.vue";

@Component({
  components: { DataTable }
})
export default class App extends Vue {
  users: any;
  search: any = "";
  componentLoaded = false;

  get filteredUsers() {
    if (!this.componentLoaded) return null;
    
    let search = this.search.toLowerCase();
    if (this.users) {
      return this.users.filter((user: any) => {
        return (
          user.username.toLowerCase().indexOf(search) !== -1 ||
          user.name.toLowerCase().indexOf(search) !== -1 ||
          user.phone.indexOf(search) !== -1 ||
          user.email.toLowerCase().indexOf(search) !== -1 ||
          user.website.toLowerCase().indexOf(search) !== -1
        );
      });
    }
  }

  mounted() {
    var vm = this;
    this.axios
      .get("https://jsonplaceholder.typicode.com/users")
      .then(response => {
        this.users = response.data;
        this.componentLoaded = true;
      });
  }
}
</script>
