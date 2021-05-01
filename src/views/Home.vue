<template>
  <div id="app">
    <h1>Todo List</h1>
      <div class="name">
        <label for="name"></label>
        <input type="text" name="name" id="name" v-model="newName" />
      </div>
      <div class="email">
        <label for="email"></label>
        <input type="email" name="email" id="email" v-model="newEmail" />
      </div>
      <button @click="insertContact">追加</button>
    <div class="table">
      <table>
        <tr v-for="item in contactLists" :key="item.id">
          <td><input type="text" v-model="item.name" /></td>
          <td><input type="email" v-model="item.email" /></td>
          <td>
            <button @click="updateContact(item.id, item.name, item.email)">
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
      const resData = await axios.get("https://blooming-caverns-24754.herokuapp.com/api/contact/");
      this.contactLists = resData.data.data;
    },
    async insertContact() {
      const sendData = {
        name: this.newName,
        email: this.newEmail,
      };
      await axios.post("https://blooming-caverns-24754.herokuapp.com/api/contact/", sendData);
      await this.getContact();
    },
    async updateContact(id, name, email) {
      const sendData = {
        name: name,
        email: email,
      };
      await axios.put("https://blooming-caverns-24754.herokuapp.com/contact/" + id, sendData);
      await this.getContact();
    },
    async deleteContact(id) {
      await axios.delete("https://blooming-caverns-24754.herokuapp.com/api/contact/" + id);
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
