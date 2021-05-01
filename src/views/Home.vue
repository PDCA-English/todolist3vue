<template>
  <div id="app">
    <h1>Todo List</h1>
      <div class="name">
        <label for="name"></label>
        <input type="text" name="name" id="name" v-model="newName" />
      </div>
      <button @click="insertContact">追加</button>
    <div class="table">
      <table>
        <tr v-for="item in contactLists" :key="item.id">
          <td><input type="text" v-model="item.name" /></td>
          <td>
            <button @click="updateContact(item.id, item.name)">
              更新
            </button>
          </td>
          <td>
            <button @click="deleteContact(item.id)">削除</button>
          </td>
        </tr>
      </table>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  data() {
    return {
      newName: "",
      newEmail: "",
      contactLists: [],
    };
  },
  methods: {
    async getContact() {
      const resData = await axios.get("http://127.0.0.1:8000/api/contact");
      this.contactLists = resData.data.data;
    },
    async insertContact() {
      const sendData = {
        name: this.newName,
      };
      await axios.post("http://127.0.0.1:8000/api/contact", sendData);
      await this.getContact();
      this.newName = "";
      this.newEmail = "";
    },
    async updateContact(id, name) {
      const sendData = {
        name: name,
      };
      await axios.put("http://127.0.0.1:8000/api/contact/" + id, sendData);
      await this.getContact();
    },
    async deleteContact(id) {
      await axios.delete("http://127.0.0.1:8000/api/contact/" + id);
      await this.getContact();
    },
  },
  created() {
    this.getContact();
  },
};
</script>

<style>
table,
td,
th {
  border: ０px none;
  border-collapse: collapse;
  text-align: center;
}
td,
th {
  padding: 5px;
}
th {
  background: #f0e6cc;
}
</style>
