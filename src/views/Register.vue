<template>
  <div class="max-w-screen-sm mx-auto px-4 py-10">
    <!-- Error Handling -->
    <div v-if="errorMsg" class="mb-10 p-4 rounded-md bg-almond shadow-lg">
      <p class="text-red-500">{{ errorMsg }}</p>
    </div>

    <!-- Registration -->
    <form
      @submit.prevent="register"
      class="p-8 flex flex-col bg-almond rounded-md shadow-lg"
    >
      <h1 class="text-3xl text-at-dark-green mb-4 self-center">Register</h1>
      <div class="flex flex-col mb-2 gap-1">
        <div class="flex mb-2 gap-8">
          <div class="flex flex-col w-1/2">
            <label for="name" class="mb-1 text-sm text-at-dark-green"
              >Name</label
            >
            <input
              type="name"
              required
              class="p-2 text-gray-500 focus:outline-none"
              id="name"
              v-model="name"
            />
          </div>
          <div class="flex flex-col w-1/2">
          <label for="lastName" class="mb-1 text-sm text-at-dark-green"
            >Last Name</label
          >
          <input
            type="lastName"
            required
            class="p-2 text-gray-500 focus:outline-none"
            id="name"
            v-model="lastName"
          />
          </div>
        </div>
        <label for="email" class="mb-1 text-sm text-at-dark-green">Email</label>
        <input
          type="email"
          required
          class="p-2 mb-2 text-gray-500 focus:outline-none"
          id="email"
          v-model="email"
        />
        <label for="password" class="mb-1 text-sm text-at-dark-green"
          >Password</label
        >
        <input
          type="password"
          required
          class="p-2 mb-2 text-gray-500 focus:outline-none"
          id="password"
          v-model="password"
        />
        <label for="confirmPassword" class="mb-1 text-sm text-at-dark-green"
          >Confirm Password</label
        >
        <input
          type="password"
          required
          class="p-2 text-gray-500 focus:outline-none"
          id="confirmPassword"
          v-model="confirmPassword"
        />
      </div>
      <button
        type="submit"
        class="mt-6 py-2 px-6 rounded-sm self-center text-sm
      text-white bg-at-olive-green duration-200 border-solid
      border-2 border-transparent hover:border-at-olive-green hover:bg-at-dark-green
      hover:text-white"
      >
        Register
      </button>

      <router-link class="text-sm mt-6 text-center" :to="{ name: 'Login' }">
        Already have an account? <span class="text-at-dark-green">Login</span>
      </router-link>
    </form>
  </div>
</template>

<script>
import { ref } from "vue";
import { supabase } from "../supabase/init";
import { useRouter } from "vue-router";

export default {
  name: "register",
  setup() {
    // Create data / vars
    const router = useRouter();
    const name = ref(null);
    const lastName = ref(null);
    const email = ref(null);
    const password = ref(null);
    const confirmPassword = ref(null);
    const errorMsg = ref(null);
    const id = ref('');

    // Register function
    const register = async () => {
      if (password.value === confirmPassword.value) {
        try {
          const { data, error } = await supabase.auth.signUp({
            // name: name.value,
            // surname: lastName.value,
            email: email.value,
            password: password.value,
          });
          if (error) throw error;
          id.value = data.user.id;
        } catch (error) {
          errorMsg.value = error.message;
          setTimeout(() => {
            errorMsg.value = null;
          }, 5000);
        }
        // Guardar en la base de datos
        try {
          console.log(`id: ${id.value}`)
          const {error} = await supabase.from('profiles').insert([{
            name: name.value,
            surname: lastName.value,
            id: id.value
          }])
          if (error) throw error;
          router.push({ name: "Login" });

        } catch(error) {
          console.log(error)
        }
        return;
      }
      errorMsg.value = "Error: Passwords do not match";
      setTimeout(() => {
        errorMsg.value = null;
      }, 5000);
    };

    return {
      email,
      password,
      confirmPassword,
      errorMsg,
      register,
      name,
      lastName,
    };
  },
};
</script>
