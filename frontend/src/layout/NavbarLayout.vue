<template>
    <v-container fluid class="w-screen d-flex align-center justify-between pl-8 bg-teal-accent-4">
        <v-row class="">
            <div class="mx-6">
                <router-link to="/" class="text-white font-weight-bold text-decoration-none">AiFIT</router-link>
            </div>
        </v-row>

        <v-col class="d-flex align-center justify-end">
            <div class="mx-6">
                <router-link to="/" class="text-white font-weight-semibold text-decoration-none">Home</router-link>
            </div>
            <div class="mx-6">
                <router-link to="/recipe/favorite"
                    class="text-white font-weight-semibold text-decoration-none">Recipe</router-link>
            </div>
            <div class="mx-6">
                <router-link to="/community"
                    class="text-white font-weight-semibold text-decoration-none">Community</router-link>
            </div>
            <div class="mx-6">
                <router-link to="/about" class="text-white font-weight-semibold text-decoration-none">About
                    us</router-link>
            </div>
            <div class="mx-6">
                <v-btn-group color="#b2d7ef" density="comfortable" rounded="pill" divided>
                    <v-btn class="bg-transparent">
                        <CircleUser class="text-white" />
                        <v-menu activator="parent" location="bottom end" transition="fade-transition">
                            <v-list density="compact" min-width="250" rounded="lg"
                                class="d-flex justify-between flex-column">
                                <router-link to="/register"
                                    class="text-teal-accent-4 underline-none font-weight-bold text-decoration-none">
                                    <v-list-item
                                        class="auth-button text-h6 rounded-lg text-teal-accent-4 underline-none font-weight-semibold text-decoration-none pt-4 mx-3"
                                        title="Register"></v-list-item>
                                </router-link>
                                <router-link to="/login"
                                    class="text-teal-accent-4 underline-none font-weight-bold text-decoration-none">
                                    <v-list-item
                                        class="auth-button text-h6 rounded-lg text-teal-accent-4 underline-none font-weight-semibold text-decoration-none pt-4 mx-3"
                                        title="Login"></v-list-item>
                                </router-link>
                                <!-- <v-list-item
                class="auth-button rounded-lg text-red-accent-1 underline-none font-weight-semibold text-decoration-none pt-4 mx-3"><button
                    @click="logout">Logout</button></v-list-item> -->
                            </v-list>
                        </v-menu>
                    </v-btn>
                </v-btn-group>
            </div>
        </v-col>
    </v-container>
</template>

<script setup>
    import { CircleUser } from 'lucide-vue-next'
</script>
<style scoped>
    .aifit-active {
        font-weight: bold;
    }

    .auth-button:hover {
        background-color: #b8f6ed;
    }
</style>
