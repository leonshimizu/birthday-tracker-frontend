<template>
  <div class="home">
    {{ message }}
    <h1>Contacts:</h1>
    {{ birthdays }}
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
      birthdays: []
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
    }
  }
};
</script>
