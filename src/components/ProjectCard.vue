<script>
export default {
    name: 'ProjectCard',
    props: {
        project: Object
    },
    data() {
        return {
            baseUrl: 'http://localhost:8000',
        }
    },
    computed: {
        contentPreview() {
            if (this.project.content.length > 50) {
                return this.project.content.substring(0, 50) + "...";
            } else {
                return this.project.content
            }
        }
    }
}
</script>

<template>
    <div class="card h-100">
        <img v-if="project.image" :src="`${baseUrl}/storage/${project.image}`" class="card-img-top" alt="project.title">
        <div v-else class="p-3">Nessuna immagine</div>
        <div class="card-body">
            <h5>{{ project.title }}</h5>
            <p>{{ contentPreview }}</p>
            <p v-if="project.type">{{ project.type.name }}</p>
            <p v-else>Nessuna tipologia</p>
            <p v-for="technology in project.technologies">{{ technology.name }}</p>
        </div>
    </div>
</template>

<style lang="scss" scoped></style>