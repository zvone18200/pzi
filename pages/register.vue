<script setup>
definePageMeta({
    layout: "custom",
});

const email = ref('');
const password = ref('');
const errorMessage = ref('');
const successMessage = ref('');

const supabase = useSupabaseClient();

const registerWithEmail = async () => {

    try {
        const { data, error } = await supabase.auth.signUp({
            email: email.value,
            password: password.value,
            options: {
                emailRedirectTo: '/',
            },
        });

        if (error) {
            errorMessage.value = error.message;
            return;
        }
        if (data) {
            successMessage.value = 'Registriran uspješno. Sada se možete prijaviti.'
        }

        errorMessage.value = '';
    } catch (error) {
        errorMessage.value = error.message;
    }
};

const registerWithFacebook = async () => {
    const { data, error } = await supabase.auth.signInWithOAuth({
        provider: "facebook",
    });

    if (error) {
        errorMessage.value = error.message;
        return;
    }


    errorMessage.value = '';

};

const registerWithTwitter = async () => {
    const { data, error } = await supabase.auth.signInWithOAuth({
        provider: "twitter",
    });

    if (error) {
        errorMessage.value = error.message;
        return;
    }

    errorMessage.value = '';

};

const registerWithGoogle = async () => {
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
            <h2 class="text-2xl font-semibold mb-6 text-center">Sign Up</h2>
            <div v-if="errorMessage" class="text-red-500 mb-4">
                {{ errorMessage }}
            </div>
            <div v-if="successMessage" class="mb-4 text-green-500">
                {{ successMessage }}
            </div>
            <form @submit.prevent="registerWithEmail">
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
                    class="w-full px-4 py-2 bg-red-400 p-3 rounded text-white font-boldrounded-md hover:bg-blue-600 transition-colors">Registriraj se</button>
            </form>
            <div class="flex justify-between items-center mt-4">
                <div class="border-b w-1/5"></div>
                <div class="text-sm text-gray-500">Ili se prijavite s</div>
                <div class="border-b w-1/5"></div>
            </div>

            <!-- Social Logins -->
            <div class="mt-4 ">
                <button @click="registerWithGoogle"
                    class="flex items-center justify-center w-auto h-10 bg-red-400 p-3 rounded-md text-white font-bold  hover:bg-blue-700 transition-colors">
                    Prijavite se putem Googlea
                </button> <br>
                <button @click="registerWithFacebook"
                    class="flex items-center justify-center w-auto h-10 bg-red-400 p-3 rounded-md text-white font-bold hover:bg-blue-700 transition-colors">
                    Prijavite se putem Facebooka
                </button>
                <br>
                <button @click="registerWithTwitter"
                    class="flex items-center justify-center w-auto h-10 bg-red-400 p-3 rounded-md text-white font-bold  hover:bg-blue-700 transition-colors">
                    Prijavite se putem X
                </button><br>
            </div>
            <div class="mt-4 text-center text-sm text-gray-600">
                Već imate račun? <a href="/login" class="text-blue-500 hover:underline">Prijavite se ovdje</a>.
            </div>
        </div>

    </div>
</template>