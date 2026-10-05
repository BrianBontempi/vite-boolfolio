<script>
export default {
    name: 'NotFoundPage'
};
</script>

<template>
    <div class="py-5 d-flex flex-column align-items-center justify-content-center">
        <h1>404 | Page not found</h1>
        <button class="btn btn-secondary mt-4" @click="$router.back()">Torna indietro</button>
    </div>
</template>

<style></style>