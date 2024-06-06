<script setup>
definePageMeta({
  layout: "custom",
});
const email = ref('');
const password = ref('');
const errorMessage = ref('');
const successMessage = ref('');
const supabase = useSupabaseClient();

const loginWithEmail = async () => {
  try {
    const { data, error } = await supabase.auth.signInWithPassword({
      email: email.value,
      password: password.value,
    });

    if (error) {
      errorMessage.value = error.message;
      return;
    }

    if (data.session) {
      navigateTo('/')
    }
  } catch (error) {
    return errorMessage.value = error.message;
  }
};

const loginWithFacebook = async () => {
  const { data, error } = await supabase.auth.signInWithOAuth({
    provider: "facebook",
  });

  if (error) {
    errorMessage.value = error.message;
    return;
  }


  errorMessage.value = '';

};

const loginWithTwitter = async () => {
  const { data, error } = await supabase.auth.signInWithOAuth({
    provider: "twitter",
  });

  if (error) {
    errorMessage.value = error.message;
    return;
  }

  errorMessage.value = '';

};

const loginWithGoogle = async () => {
  const { data, error } = await supabase.auth.signInWithOAuth({
    provider: "google",
  });

  if (error) {
    errorMessage.value = error.message;
    return;
  }


  errorMessage.value = '';

};
</script>

<template>
  <div class="min-h-screen flex items-center justify-center bg-gray-100">
    <div class="max-w-md w-full p-8 bg-white shadow-lg rounded-md">
      <h2 class="text-2xl font-semibold mb-6 text-center">Prijaviti se</h2>
      <div v-if="errorMessage" class="text-red-500 mb-4">
        {{ errorMessage }}
      </div>
      <div v-if="successMessage" class="text-green-500 mb-4">
        {{ successMessage }}
      </div>

      <form @submit.prevent="loginWithEmail">
        <div class="mb-4">
          <label for="email" class="block text-sm font-medium text-gray-700">Email adresa</label>
          <input type="email" id="email" v-model="email"
            class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-300 focus:ring focus:ring-blue-200 focus:ring-opacity-50">
        </div>

        <div class="mb-4">
          <label for="password" class="block text-sm font-medium text-gray-700">Lozinka</label>
          <input type="password" id="password" v-model="password"
            class="mt-1 block w-full rounded-md border-gray-300 shadow-sm focus:border-blue-300 focus:ring focus:ring-blue-200 focus:ring-opacity-50">
        </div>
        <button type="submit"
          class="w-full px-4 py-2 bg-red-400 p-3 rounded text-white font-boldrounded-md hover:bg-blue-600 transition-colors">Prijaviti
          se</button>
      </form>
      <div class="flex justify-between items-center mt-4">
        <div class="border-b w-1/5"></div>
        <div class="text-sm text-gray-500">Ili se prijavite sa</div>
        <div class="border-b w-1/5"></div>
      </div>

      <!-- Social Logins -->
      <div class="mt-4 ">
        <button @click="loginWithGoogle"
          class="flex items-center justify-center w-auto h-10 bg-red-400 p-3 rounded-md text-white font-bold  hover:bg-blue-700 transition-colors">
          Prijavite se s Googlea
        </button> <br>
        <button @click="loginWithFacebook"
          class="flex items-center justify-center w-auto h-10 bg-red-400 p-3 rounded-md text-white font-bold hover:bg-blue-700 transition-colors">
          Prijavite se s Facebooka
        </button>
        <br>
        <button @click="loginWithTwitter"
          class="flex items-center justify-center w-auto h-10 bg-red-400 p-3 rounded-md text-white font-bold  hover:bg-blue-700 transition-colors">
          Prijavite se s X
        </button><br>
      </div>
      <div class="mt-4 text-center text-sm text-gray-600">
        Nemam račun? <a href="/register" class="text-blue-500 hover:underline">Registriraj se</a>.
      </div>
    </div>

  </div>
</template>
