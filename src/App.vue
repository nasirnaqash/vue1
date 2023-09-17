<template>
  <div class="card">
    <form @submit.prevent="submitForm">
      <div class="form-group">
        <label for="name">Name:</label>
        <input
          type="text"
          id="name"
          v-model="formData.name"
          placeholder="Enter your name"
        />
        <span class="error" v-if="errors.name">{{ errors.name }}</span>
      </div>
      <div class="form-group">
        <label for="email">Email:</label>
        <input
          type="email"
          id="email"
          v-model="formData.email"
          placeholder="Enter your email"
        />
        <span class="error" v-if="errors.email">{{ errors.email }}</span>
      
      </div>
      <div class="form-group">
        <button type="submit">Submit</button>
      <button type="button" @click="resetForm">Clear</button>
      </div>

      
    </form>

    <div v-if="submittedData">
      <h2>Form Submission Results</h2>
      <p>Name: {{ submittedData.name }}</p>
      <p>Email: {{ submittedData.email }}</p>
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      formData: {
        name: '',
        email: '',
      },
      errors: {},
      submittedData: null,
    };
  },
  methods: {
    submitForm() {
      this.errors = {};

      if (!this.validateForm()) {
        return;
      }

      axios
        .post('https://jsonplaceholder.typicode.com/posts', this.formData)
        .then((response) => {
          this.submittedData = response.data;
        })
        .catch((error) => {
          console.error('API Error:', error);
          this.errors.submit = 'An error occurred while submitting the form.';
        });
    },
    resetForm() {
      // Clear form data
      this.formData = {
        name: '',
        email: '',
        // Clear other form fields as needed
      };
      this.submittedData = null;
    },
    validateForm() {
      
      let isValid = true;

      if (!this.formData.name.trim()) {
        this.errors.name = 'Name is required.';
        isValid = false;
      }

      // Validate Email
      if (!this.formData.email.trim()) {
        this.errors.email = 'Email is required.';
        isValid = false;
      } else if (
        !/^\w+([.-]?\w+)*@\w+([.-]?\w+)*(\.\w{2,4})+$/.test(
          this.formData.email
        )
      ) {
        this.errors.email = 'Invalid email format.';
        isValid = false;
      }


      return isValid;
    },
  },
};
</script>

<style >
.form-group {
  
  margin-bottom: 15px;
  padding: 10px;
  display: grid;
  place-items: center;
  
}

.error {
  color: red;
  font-size: 14px;
  margin-top: 5px;
}
:root {
  font-size: 20px;
  box-sizing: inherit;
}

*,
*:before,
*:after {
  box-sizing: inherit;
}

p {
  margin: 0;
}

p:not(:last-child) {
  margin-bottom: 1.5em;
}

body {
  font-family: "Inter", sans-serif;

  display: flex;
  align-items: center;
  justify-content: center;
  min-height: 100vh;
  padding: 30px;
  
  color: #224;
  background-image: url("https://source.unsplash.com/E8Ufcyxz514/2400x1823");
background-position: center;
background-size: cover;
background-repeat: no-repeat;
background-attachment: fixed;
}

.card {
  max-width: 300px;
  min-height: 200px;
  display: flex;
  flex-direction: column;
  justify-content: space-between;

  max-width: 500px;
  height: fit-content;
  padding: 35px;

  border: 1px solid rgba(255, 255, 255, .25);
  border-radius: 20px;
  background-color: rgba(255, 255, 255, 0.45);
  box-shadow: 0 0 10px 1px rgba(0, 0, 0, 0.25);

  backdrop-filter: blur(15px);
}

button {
  background-color: rgba(0, 0, 0, 0); /* Transparent black (0% opacity) */
  /* border: 1px solid #000; Add a border for visibility (optional) */
  color: #000; /* Text color */
  padding: 10px 20px; /* Adjust padding as needed */
  cursor: pointer; /* Add a pointer cursor for interactivity */
  border-radius: 8px;
  margin-bottom: 3px;
}
input{
  margin-top: 5px;
  color: #fff;
  border: 1px black solid;
  padding: 10px 20px;
  border-radius: 8px;
  cursor: pointer;
}

</style>
