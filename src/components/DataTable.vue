<template>
  <table></table>
</template>

<script lang="ts">
import { Component, Vue, Prop, Watch } from "vue-property-decorator";
import $ from "jquery"; 
require('imports-loader?define=>false!datatables.net')(window, $);

@Component({
  components: {}
})
export default class DataTable extends Vue {
  @Prop() users: any;
  headers: any = [
    { title: "ID" },
    { title: "Username", class: "some-special-class" },
    { title: "Real Name" },
    { title: "Phone" },
    { title: "Email" },
    { title: "Website" }
  ];
  rows: any = [];
  dtHandle!: any;

  @Watch("users", { immediate: true, deep: true })
  onUsersChanged(val: any, oldVal: any) {
    //alert(3)
    this.rows = [];
    // You should _probably_ check that this is changed data... but we'll skip that for this example.
    val.forEach((item: any) => {
      // Fish out the specific column data for each item in your data set and push it to the appropriate place.
      // Basically we're just building a multi-dimensional array here. If the data is _already_ in the right format you could
      // skip this loop...
      let row = [];

      row.push(item.id);
      row.push(item.username);
      row.push(item.name);
      row.push(item.phone);
      row.push('<a href="mailto://' + item.email + '">' + item.email + "</a>");
      row.push(
        '<a href="http://' +
          item.website +
          '" target="_blank">' +
          item.website +
          "</a>"
      );

      this.rows.push(row);
    });

    // Here's the magic to keeping the DataTable in sync.
    // It must be cleared, new rows added, then redrawn!
    this.dtHandle.clear();
    this.dtHandle.rows.add(this.rows);
    this.dtHandle.draw();
  }

  mounted() {
    // Instantiate the datatable and store the reference to the instance in our dtHandle element.
    this.dtHandle = ($(this.$el) as any).DataTable({
      // Specify whatever options you want, at a minimum these:
      columns: this.headers,
      data: this.rows,
      searching: false,
      paging: false,
      info: false
    });
  }
}
</script>