<script>
import { RouterLink } from 'vue-router';
</script>

<template>
    <nav class="nav">
        <RouterLink active-class="active" class="nav-item" to="/">Home</RouterLink>
        <RouterLink active-class="active" class="nav-item" to="/about">About</RouterLink>
    </nav>
</template>

<style scoped>
.nav {
    display: flex;
    justify-content: space-around;
    background-color: #a8a2a2;
    padding: 10px;
}

.nav-item {
    list-style: none;
    text-decoration: none;
    color: white;
    font-size: 18px;
}

.active {
    font-weight: bold;
}
</style>