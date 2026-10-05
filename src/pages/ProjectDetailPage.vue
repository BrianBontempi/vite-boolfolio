<script>
import axios from 'axios';
import ProjectCard from '../components/projects/ProjectCard.vue';
import AppLoader from '../components/AppLoader.vue';
const endpoint = 'http://localhost:8000/api/projects/';
export default {
    name: 'ProjectDetailPage',
    components: { ProjectCard, AppLoader },
    data: () => ({
        project: null,
        isLoading: false,
        hasError: false
    }),
    methods: {
        getProject() {
            this.isLoading = true;
            this.hasError = false;
            axios.get(endpoint + this.$route.params.slug)
                .then(res => {
                    this.project = res.data;
                })
                .catch(err => {
                    // se il progetto non esiste vado alla pagina 404
                    if (err.response && err.response.status === 404) {
                        this.$router.replace({ name: 'not-found' });
                        return;
                    }
                    console.error(err);
                    this.hasError = true;
                })
                .then(() => {
                    this.isLoading = false;
                })
        }
    },
    created() {
        this.getProject();
    }
};
</script>

<template>
    <AppLoader v-if="isLoading" />
    <div v-else-if="hasError" class="alert alert-danger my-5">Impossibile caricare il progetto, riprova più tardi.</div>
    <ProjectCard v-else-if="project" :project="project" :isDetail="true" />
</template>

<style></style>