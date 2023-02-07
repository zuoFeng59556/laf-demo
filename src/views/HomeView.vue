<script setup>
import { Cloud } from "laf-client-sdk";
import { ref } from "vue";

const cloud = new Cloud({
  baseUrl: "http://pcsw70.preview.laf.run",
  getAccessToken: () => '',
});

// =====================================data=====================================
const list = ref([])
const value = ref('')

// =====================================function=====================================
getList()

async function getList() {
  const res = await cloud.invoke('get-list')
  list.value = res
}

async function add() {
  const res = await cloud.invoke('add-item', { name: value.value })
  if (res) {
    value.value = ''
    getList()
  }
}

async function remove(id) {
  const res = await cloud.invoke('delete-item', { id })
  if (res) getList()
}
</script>

<template>
  <div class="box">
    <input v-model="value" type="text">
    <button class="addButton" @click="add">添加</button>
    <ul>
      <div class="itemBox" v-for="item in list" :key="item._id">
        <li class="itemName">{{ item.name }}</li>
        <button @click="remove(item._id)">移除</button>
      </div>
    </ul>
  </div>
</template>

<style  scoped>
.box {
  margin: 10% 0 0 40%;
  width: 100%;
}

.addButton {
  margin-left: 10px;
}

.itemBox {
  display: flex;
  margin-top: 10px;
}

.itemName {
  width: 100px;
}
</style>
