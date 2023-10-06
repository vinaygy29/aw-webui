<template>
  <div id="signup">
    <h1>Signup</h1>
    <div class="form-inputs">
      <label for="username">Username</label>
      <input
        id="username"
        v-model="input.username"
        type="text"
        name="username"
        placeholder="Username"
      />
    </div>
    <div class="form-inputs">
      <label for="password">Password</label>
      <input
        id="password"
        v-model="input.password"
        type="password"
        name="password"
        placeholder="Password"
      />
    </div>
    <button type="button" @click="signup()">Signup</button><br />
    <a type="button" @click="$router.replace('/login')">Have an account? Login</a>
  </div>
</template>

<script>
export default {
  name: 'Signup',
  data() {
    return {
      input: {
        username: '',
        password: '',
      },
    };
  },

  created() {
    localStorage.removeItem('token');
  },
  methods: {
    signup() {
      if (this.input.username != '' && this.input.password != '') {
        // This should actually be an api call not a check against this.$parent.mockAccount
        // Define the URL of your API endpoint
        const apiUrl = 'http://127.0.0.1:5600/api/0/user'; // Adjust the URL as needed

        // Create an object with the request data (username and password)
        const requestData = {
          user_name: this.input.username, // Replace with the actual username
          password: this.input.password, // Replace with the actual password
        };

        // Define the options for the fetch POST request
        const requestOptions = {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(requestData),
        };

        // Make the POST request to the API
        fetch(apiUrl, requestOptions)
          .then(response => {
            return response.json(); // Parse the response JSON
          })
          .then(data => {
            this.$emit('account created', true);
            alert(data?.message);
            this.$router.replace('/');
          })
          .catch(error => {
            console.error('There was a problem with the fetch operation:', error);
          });
      } else {
        alert('A username and password must be present');
      }
    },
  },
};
</script>

<style>
#signup .form-inputs {
  padding-bottom: 10px;
}

#signup .form-inputs label {
  padding-right: 10px;
}
</style>
