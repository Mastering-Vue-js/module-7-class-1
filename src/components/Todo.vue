<script setup>
import { ref, reactive } from "vue";

const users = reactive([]);
const searchInput = ref("");

const getUsersFromServer = () => {
  fetch("https://jsonplaceholder.typicode.com/users")
    .then((res) => res.json())
    .then((data) => {
      data.forEach((user) => {
        users.push({ name: user.name, email: user.email, username: user.username });
      });
    });
};

getUsersFromServer();

const clearSearch = () => {
  searchInput.value = "";
};

const getUsers = () => {
  if (searchInput.value) {
    return users.filter((user) => {
      if (user.name.toLowerCase().includes(searchInput.value.toLowerCase()) || user.email.toLowerCase().includes(searchInput.value.toLowerCase()) || user.username.toLowerCase().includes(searchInput.value.toLowerCase())) {
        return user;
      }
    });
  } else {
    return users;
  }
};

const currentlyEditing = ref('');
const editUser = (index) => {
    currentlyEditing.value = index;
    console.log(currentlyEditing.value);
}

const submit = () => {
    console.log(currentlyEditing.value);
}

</script>

<template>
  <input type="text" placeholder="Search" v-model="searchInput" /><button
    @click="clearSearch()"
  >
    clear
  </button>
  <!-- <p>Writting: {{ searchInput }}</p> -->
  <ul>
    <li v-for="(user, index) in getUsers()" :key="index">
      Name: {{ user.name }} - Email: {{ user.email }} - Username: {{ user.username }}
      <button @click="editUser(index)">Edit</button>
    </li>
    <li><button @click="submit()">Submit</button></li>
  </ul>
</template>

<style scoped>
ul li {
  list-style: none;
}
</style>
