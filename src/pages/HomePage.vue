<script>
import ProjectsList from '../components/projects/ProjectsList.vue';
import AppLoader from '../components/AppLoader.vue';
import axios from 'axios';
const endpoint = 'http://localhost:8000/api/projects/';
export default {
    name: 'HomePage',
    components: { ProjectsList, AppLoader },
    data: () => ({
        projects: [],
        pagination: { currentPage: 1, lastPage: 1 },
        isLoading: false,
        hasError: false
    }),
    methods: {
        fetchProjects(page = 1) {
            this.isLoading = true;
            this.hasError = false;
            axios.get(endpoint, { params: { page } })
                .then(res => {
                    const { data, current_page, last_page } = res.data;
                    this.projects = data;
                    this.pagination = { currentPage: current_page, lastPage: last_page };
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
    <template v-else>
        <ProjectsList :projects="projects" />
        <nav v-if="pagination.lastPage > 1" class="d-flex justify-content-between align-items-center mb-5">
            <button class="btn btn-outline-primary" :disabled="pagination.currentPage === 1"
                @click="fetchProjects(pagination.currentPage - 1)">Precedente</button>
            <span>Pagina {{ pagination.currentPage }} di {{ pagination.lastPage }}</span>
            <button class="btn btn-outline-primary" :disabled="pagination.currentPage === pagination.lastPage"
                @click="fetchProjects(pagination.currentPage + 1)">Successiva</button>
        </nav>
    </template>

</template>

<style scoped></style>
