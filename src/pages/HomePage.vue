<script>
import ProjectsList from '../components/projects/ProjectsList.vue';
import AppLoader from '../components/AppLoader.vue';
import axios from 'axios';
const endpoint = 'http://localhost:8000/api/projects/';
export default {
    name: 'HomePage',
    components: { ProjectsList, AppLoader },
    data: () => ({ projects: [], isLoading: false, hasError: false }),
    methods: {
        fetchProjects() {
            this.isLoading = true;
            this.hasError = false;
            axios.get(endpoint)
                .then(res => {
                    this.projects = res.data;
                })
                .catch(err => {
                    console.error(err);
                    this.hasError = true;
                })
                .then(() => {
                    this.isLoading = false;
                })
        }
    },
    created() {
        this.fetchProjects();
    }
};
</script>

<template>

    <h1>Boolfolio</h1>
    <AppLoader v-if="isLoading" />
    <div v-else-if="hasError" class="alert alert-danger my-5">Impossibile caricare i progetti, riprova più tardi.</div>
    <ProjectsList v-else :projects="projects" />

</template>

<style scoped></style>