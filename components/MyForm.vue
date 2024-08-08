<template>
  <div class="max-w-md mx-auto bg-white p-8 rounded-lg shadow-lg">
    <form @submit.prevent="handleSubmit" class="space-y-6 max-w-[400px] min-w-[300px]">
      <FormInput
        label="Name"
        id="name"
        v-model="form.name"
        :error="errors.name"
        :validate="validateName"
      />
      <FormInput
        label="Email"
        id="email"
        type="email"
        v-model="form.email"
        :error="errors.email"
        :validate="validateEmail"
      />
      <FormInput
        label="Password"
        id="password"
        type="password"
        v-model="form.password"
        :error="errors.password"
        :validate="validatePassword"
      />
      <FormInput
        label="Confirm Password"
        id="confirmPassword"
        type="password"
        v-model="form.confirmPassword"
        :error="errors.confirmPassword"
        :validate="validateConfirmPassword"
      />
      <div>
        <button
          :disabled="isLoading"
          type="submit"
          class="w-full flex justify-center py-2 px-4 border border-transparent rounded-md shadow-sm text-sm font-medium text-white bg-indigo-600 hover:bg-indigo-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-indigo-500"
        >
          <span v-if="isLoading">Loading...</span>
          <span v-else>Submit</span>
        </button>
      </div>
    </form>
  </div>
</template>

<script>
import FormInput from './reusables/FormInput.vue';

export default {
  components: {
    FormInput,
  },
  data() {
    return {
      form: {
        name: '',
        email: '',
        password: '',
        confirmPassword: '',
      },
      isLoading: false,
      errors: {},
    };
  },
  methods: {
    validateName() {
      if (!this.form.name) {
        this.errors.name = 'Name is required';
      } else {
        this.errors.name = '';
      }
    },
    validateEmail() {
      const emailPattern = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
      if (!this.form.email) {
        this.errors.email = 'Email is required';
      } else if (!emailPattern.test(this.form.email)) {
        this.errors.email = 'Email must be valid';
      } else {
        this.errors.email = '';
      }
    },
    validatePassword() {
      if (!this.form.password) {
        this.errors.password = 'Password is required';
      } else if (this.form.password.length < 6) {
        this.errors.password = 'Password must be at least 6 characters';
      } else {
        this.errors.password = '';
      }
    },
    validateConfirmPassword() {
      if (!this.form.confirmPassword) {
        this.errors.confirmPassword = 'Please confirm your password';
      } else if (this.form.confirmPassword !== this.form.password) {
        this.errors.confirmPassword = 'Passwords do not match';
      } else {
        this.errors.confirmPassword = '';
      }
    },
    async handleSubmit() {
      this.validateName();
      this.validateEmail();
      this.validatePassword();
      this.validateConfirmPassword();

      if (!this.errors.name && !this.errors.email && !this.errors.password && !this.errors.confirmPassword) {
        try {
          this.isLoading = true;
          console.log(this.isLoading);
          const response = await fetch('https://jsonplaceholder.typicode.com/posts', {
            method: 'POST',
            headers: {
              'Content-Type': 'application/json',
            },
            body: JSON.stringify(this.form),
          });
          const data = await response.json();
          this.resetForm();
        } catch (error) {
          console.error('Error calling API:', error);
        }
      }
    },
    resetForm() {
      this.form.name = '';
      this.form.email = '';
      this.form.password = '';
      this.form.confirmPassword = '';
      this.isLoading = false;
      this.errors = {};
    },
  },
};
</script>
