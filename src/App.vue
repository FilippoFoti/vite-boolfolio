<script>
import axios from 'axios';
import ProjectCard from './components/ProjectCard.vue';

export default {
  data() {
    return {
      baseUrl: "http://localhost:8000",
      projects: []
    };
  },
  mounted() {
    this.getProjects();
  },
  methods: {
    getProjects() {
      axios.get(`${this.baseUrl}/api/projects`).then(resp => {
        this.projects = resp.data.results;
      });
    }
  },
  components: { ProjectCard }
}
</script>

<template>
  <div class="container">
    <h2>Lista dei Progetti</h2>
    <div class="row row-cols-3 g-3">
      <div class="col" v-for="project in projects" :key="project.id">
        <ProjectCard :project="project" />
      </div>
    </div>
  </div>
</template>

<style lang="scss">
@use "./styles/general.scss" as *;
</style>