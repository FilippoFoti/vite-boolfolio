<script>
import axios from 'axios';
import ProjectCard from './components/ProjectCard.vue';

export default {
  data() {
    return {
      baseUrl: "http://localhost:8000",
      projects: [],
      currentPage: 1,
      lastPage: null,
      totalProjects: 0,
      loading: false,
    };
  },
  mounted() {
    this.getProjects();
  },
  methods: {
    // getProjects() {
    //   axios.get(`${this.baseUrl}/api/projects`).then(resp => {
    //     this.projects = resp.data.results.data;
    //   });
    // }
    getProjects(pageNumber = 1) {
      this.loading = true;
      axios.get(`${this.baseUrl}/api/projects`, {
        params: {
          page: pageNumber
        }
      }).then(resp => {
        this.projects = resp.data.results.data;
        this.currentPage = resp.data.results.current_page;
        this.lastPage = resp.data.results.last_page;
        this.totalProjects = resp.data.results.total;
      }).finally(() => {
        this.loading = false;
      })
    }
  },
  components: { ProjectCard }
}
</script>

<template>
  <div class="container">
    <div v-if="!loading">
      <h2 class="m-0 py-3">Lista dei Progetti</h2>
      <div class="text-start">
        <p>Trovati {{ totalProjects }} progetti</p>
      </div>
      <div class="row row-cols-3 g-3">
        <div class="col" v-for="project in projects" :key="project.id">
          <ProjectCard :project="project" />
        </div>
      </div>

      <nav v-if="lastPage" class="m-4 d-flex justify-content-center" aria-label="Page navigation example">
        <ul class="pagination">
          <li class="page-item" :class="{ 'disabled': currentPage === 1 }"><a
              @click.prevent="getProjects(currentPage - 1)" class="page-link" href="#">Previous</a></li>
          <li class="page-item" :class="{ 'active': pageNumber === currentPage }" v-for="pageNumber in lastPage"><a @click.prevent="getProjects(pageNumber)" class="page-link" href="#">{{ pageNumber }}</a></li>
          <li class="page-item" :class="{ 'disabled': currentPage === lastPage }"><a
              @click.prevent="getProjects(currentPage + 1)" class="page-link" href="#">Next</a></li>
        </ul>
      </nav>

    </div>
    <div v-else>
      <p>Loading...</p>
    </div>
  </div>
</template>

<style lang="scss">
@use "./styles/general.scss" as *;
</style>