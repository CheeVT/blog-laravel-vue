<template>
    <div class="max-w-md mx-auto w-full">
        <form class="mt-8 space-y-6" v-on:submit.prevent="login">
            <div class="rounded-md shadow-sm space-y-4">
                <div>
                    <label for="email" class="block text-sm font-medium text-gray-700">Email</label>
                    <div class="mt-1">
                        <input type="email" name="email" id="email" class="shadow-sm focus:ring-indigo-500
                        focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md"
                        placeholder="email@gmail.com" v-model="loginForm.email">
                    </div>
                </div>
                <div>
                    <label for="password" class="block text-sm font-medium text-gray-700">Password</label>
                    <div class="mt-1">
                        <input type="password" name="password" id="password" class="shadow-sm focus:ring-indigo-500
                        focus:border-indigo-500 block w-full sm:text-sm border-gray-300 rounded-md"
                        placeholder="******" v-model="loginForm.password">
                    </div>
                </div>
                <button type="submit" class="group relative w-full flex justify-center py-2 px-4 border border-transparent
                text-sm font-medium rounded-md text-white bg-blue-600 hover:bg-blue-700 focus:outline-none focus:ring-2
                focus:ring-offset-2 focus:ring-blue-500">Login</button>
            </div>
        </form>
    </div>
</template>

<script>
import { useStore } from 'vuex';
import { reactive } from 'vue';

export default {
    setup() {
        const store = useStore();

        const loginForm = reactive({
            email: '',
            password: ''
        });

        const login = () => {
            store.dispatch('login', loginForm)
        }

        return {
            login,
            loginForm
        }

    }
}
</script>