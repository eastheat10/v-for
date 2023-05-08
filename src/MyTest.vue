<template>
  <div>
    <div>
      <h1>v-bind:key="None"</h1>
      <table>
        <thead>
          <tr>
            <th>None</th>
            <th>Name</th>
            <th>Value</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="fruit in fruits0">  <!-- eslint 빨간줄 -->
            <td> - </td>
            <td>{{ fruit.name }}</td>
            <td>
              <input type="number" />
            </td>
          </tr>
        </tbody>
      </table>
      <button v-on:click="shift0">shift</button>
    </div>
    <br /><br /><br />

    <div>
      <h1>v-bind:key="Array Index"</h1>
      <table>
        <thead>
          <tr>
            <th>Index</th>
            <th>Name</th>
            <th>Value</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(fruit, i) in fruits1" :key="i">
            <td>{{ i }}</td>
            <td>{{ fruit.name }}</td>
            <td>
              <input type="number" />
            </td>
          </tr>
        </tbody>
      </table>
      <button v-on:click="shift1">shift</button>
    </div>
    <br /><br /><br />

    <div>
      <h1>v-bind:key="Object Id"</h1>
      <table>
        <thead>
          <tr>
            <th>Object Id</th>
            <th>Name</th>
            <th>Value</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="fruit in fruits2" :key="fruit.id">
            <td>{{ fruit.id }}</td>
            <td>{{ fruit.name }}</td>
            <td><input type="number" /></td>
          </tr>
        </tbody>
      </table>
      <button v-on:click="shift2">shift</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const fruits0 = ref([
  { id: 1, name: "apple" },
  { id: 2, name: "orange" },
  { id: 3, name: "melon" },
  { id: 4, name: "grape" },
  { id: 5, name: "mango" }
]);

const fruits1 = ref([
  { id: 1, name: "apple" },
  { id: 2, name: "orange" },
  { id: 3, name: "melon" },
  { id: 4, name: "grape" },
  { id: 5, name: "mango" }
]);

const fruits2 = ref([
  { id: "a", name: "apple" },
  { id: "b", name: "orange" },
  { id: "c", name: "melon" },
  { id: "d", name: "grape" },
  { id: "e", name: "mango" }
]);

const shift0 = () => {
  fruits0.value.push(fruits0.value.shift());
};

const shift1 = () => {
  fruits1.value.push(fruits1.value.shift());
};

const shift2 = () => {
  fruits2.value.push(fruits2.value.shift());
};
</script>

<style>
table {
    border-collapse: collapse;
}

th {   
    border: 1px solid #ccc;
}

td {
    border: 1px solid #ccc;
}
</style>
