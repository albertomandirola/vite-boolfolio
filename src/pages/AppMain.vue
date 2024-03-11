<script>
import ProjectCard from '../components/ProjectCard.vue';
import { store } from '../store.js';
import axios from 'axios';
export default {
    name: 'AppMain',
    data() {
        return {
            store,
            projects: [],
            current_page: 1,
            lastPage: null
        }
    },
    components: {
        ProjectCard
    },
    created() {
        this.getProjects();
    },
    methods: {
      getProjects(page_num) {
            axios.get(`${this.store.baseUrl}/api/projects`, {
                params: {
                    page: page_num
                }
            }).then((response) => {
                console.log(response);
                this.projects = response.data.project.data;
                this.currentPage = response.data.project.current_page;
                this.lastPage = response.data.project.last_page;
    
            })
        }
    },
}
</script>
<template lang="">
    
    <main>
        <div class="container">
            <div class="row">
                <ProjectCard v-for="project, index in projects" :key="index" :project="project"/>
            </div>
        </div>
        <div class="row">
            <div class="col-12 d-flex justify-content-center ">
                <ul class="pagination mt-4">
                    <li>
                        <button :class="currentPage == 1 ? 'disabled' : ''" class="btn btn-sm btn-square btn-page mx-2" @click="getProjects(currentPage - 1)">
                            <i class="fas fa-arrow-left"></i>
                        </button>
                    </li>
                    <li>
                        <button :class="currentPage == lastPage ? 'disabled' : ''" class="btn btn-sm btn-square btn-page" @click="getProjects(currentPage + 1)">
                            <i class="fas fa-arrow-right"></i>
                        </button>
                    </li>
                </ul>
            </div>
        </div>
    </main>
    
</template>
<style lang="scss">
    @use 'src/style/general.scss'
</style>