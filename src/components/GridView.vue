
<template>
    <div>
      <table>
        <tr>
          <th v-for="label in header" v-bind:key="label.id" v-html="label"></th>
        </tr>
        <RowView
          v-for="item in content"
          v-bind:content="item"
          v-bind:key="item.id"
        ></RowView>
      </table>
    </div>
  </template>
  
  <script>
  import RowView from "./RowView.vue";
  
  export default {
    name: "GridView",
    components: { RowView },
    props: ["header", "content"],
  };
  </script>
  
  <style src="./grid.css"></style>
  