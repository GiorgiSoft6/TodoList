<template>
    <div>
        <h2>{{ route.params.test }}</h2>
    </div>
</template>

<script setup>
import { useRoute } from 'vue-router';
const route = useRoute();

console.log(route);

</script>