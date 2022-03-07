<template>
    <div>
        <div v-for="(item, id) in repolist" :key="id">
               <ul class="list-group">
                    <li class="list-group-item d-flex justify-content-between align-items-center">
                        <p> <abbr :title="item.description"> {{item.full_name}} </abbr>  </p>
                        <span class="badge badge-primary badge-pill">{{ item.forks_count}} <img src="imagen_fork.png" style="width:8px"></span>
                    </li>
                </ul>
        </div>
    </div>
</template>

<script>
export default {
    name: 'GitHubRepos',
    props: [
        'repolist'
    ],
}
</script>

<style scoped>
</style>
