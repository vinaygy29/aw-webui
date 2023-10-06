<template>
  <div id="login">
    <h1>Login</h1>
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
    <button type="button" @click="login()">Login</button><br />
    <a type="button" @click="$router.replace('/signup')">Don not have an account? Signup</a>
  </div>
</template>

<script>
export default {
  name: 'Login',
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
    login() {
      if (this.input.username != '' && this.input.password != '') {
        // This should actually be an api call not a check against this.$parent.mockAccount
        // Define the URL of your API endpoint
        const apiUrl = 'http://127.0.0.1:5600/api/0/login'; // Adjust the URL as needed

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
            if (!response.ok) {
              if (response.status === 401) {
                alert('The username and / or password is incorrect');
              }
            }
            return response.json(); // Parse the response JSON
          })
          .then(data => {
            if (data?.token) {
              this.$emit('authenticated', true);
              localStorage.setItem('validLogin', true);
              localStorage.setItem('token', data?.token);
              this.$router.replace('/home');
            } else {
              alert(data?.message);
            }
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
#login .form-inputs {
  padding-bottom: 10px;
}

#login .form-inputs label {
  padding-right: 10px;
}
</style>
