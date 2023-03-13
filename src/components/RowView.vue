<template>
    <tr>
      <td v-for="item in content" v-bind:key="item.id" v-html="item"></td>
    </tr>
  </template>
  
  <script>
  export default {
    name: "RepositoryI",
    props: ["content"],
  };
  </script>
  