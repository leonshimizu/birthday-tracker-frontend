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
        <button v-on:click="createFunction()">Add</button>
        <button>Close</button>
      </form>
    </dialog>
    <ul>
      <li v-for="birthday in birthdays" :key="birthday.id">
        {{ birthday.id }}
        {{ birthday.name }}
        <button v-on:click="showFunction(birthday)">Show More Info</button>
      </li>
    </ul>
    <dialog id="show-modal">
      <form mehod="dialog">
        <!-- <p>Name: {{ currentBirthday.name }}</p>
        <p>Birthday: {{ currentBirthday.month }}/{{ currentBirthday.day }}/{{ currentBirthday.year }}</p> -->
        <p>Name: <input type="text" v-model="currentBirthday.name"></p>
        <p>Month: <input type="text" v-model="currentBirthday.month"></p>
        <p>Day: <input type="text" v-model="currentBirthday.day"></p>
        <p>Year: <input type="text" v-model="currentBirthday.year"></p>
        <button v-on:click="updateFunction(currentBirthday)">Update</button>
        <button v-on:click="deleteFunction(currentBirthday)">Delete</button>
        <button>Close</button>
      </form>
    </dialog>
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
      currentBirthday: {}
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
    },
    showFunction: function(theBirthday) {
      console.log("in the show function");
      document.querySelector("#show-modal").showModal();
      this.currentBirthday = theBirthday;
      console.log(this.currentBirthday.name)
    },
    updateFunction: function(theBirthday) {
      console.log("in the update function");
      axios
        .patch(`http://localhost:3000/birthdays/${theBirthday.id}`, {
          name: this.currentBirthday.name,
          month: this.currentBirthday.month,
          day: this.currentBirthday.day,
          year: this.currentBirthday.year
        })
        .then(response => {
          console.log(response.data);
        })
    },
    deleteFunction: function(theBirthday) {
      // console.log("in the delete function");
      // console.log(theBirthday.name);
      var index = this.birthdays.indexOf(theBirthday);
      this.birthdays.splice(index, 1);
      axios
        .delete(`http://localhost:3000/birthdays/${theBirthday.id}`)
        .then(response => {
          console.log(response.data);
        })
    }
  }
};
</script>
