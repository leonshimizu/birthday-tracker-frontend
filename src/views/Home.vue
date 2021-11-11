<template>
  <div class="home">
    {{ message }}
    <h1>Contacts:</h1>
    <button v-on:click="createModal()">New birthday</button>
    <dialog id="create-modal">
      <form type="dialog">
        <input type="text" v-model="name">
        <input type="text" v-model="month">
        <input type="text" v-model="day">
        <input type="text" v-model="year">
        <button v-on:click="createFunction">Add</button>
        <button>Close</button>
      </form>
    </dialog>
    <ul>
      <li v-for="birthday in birthdays" :key="birthday.id">
        {{ birthday.id }}
        {{ birthday.name }}
      </li>
    </ul>
    <!-- {{ birthdays }} -->
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: "Home",
  components: {},
  data: function() {
    return {
      message: "Hello World",
      birthdays: [],
      name: "Name",
      month: "Month",
      day: "Day",
      year: "Year",
      newBirthday: {}
    }
  },
  created: function() {
    this.indexFunction();
  },
  methods: {
    indexFunction: function() {
      console.log("in the index function");
      axios
        .get('http://localhost:3000/birthdays')
        .then(response => {
          console.log(response.data);
          this.birthdays = response.data;
        })
    },
    createModal: function() {
      console.log("in the create modal");
      document.querySelector("#create-modal").showModal();
    },
    createFunction: function() {
      console.log("in the create function");
      axios
        .post('http://localhost:3000/birthdays', {
          name: this.name,
          month: this.month,
          day: this.day,
          year: this.year
        })
        .then(response => {
          console.log(response.data);
          this.birthdays.push(response.data);
        })
    }
  }
};
</script>
