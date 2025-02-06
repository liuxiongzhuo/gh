<script setup>
import { ref, watch } from 'vue';
const props = defineProps(['owner'])
const repos = ref([])
const repoNum = ref(0)
const currentPage = ref(Number(new URL(window.location.href).searchParams.get('page')) ?? 1)
fetch('http://ghapi.0031400.xyz/' + props.owner + '?page=' + currentPage.value)
    .then(res => res.json())
    .then(data => {
        if (data == null) {
            window.location.href = '/404'
        } else {
            repos.value = data.repos
            repoNum.value = Number(data.repoNum)
        }
    })
watch(currentPage, (num) => {
    window.location.search = '?page=' + num
})
</script>
<template>
    <div id="container">
        <div id="main">

            <el-card>{{ props.owner }}</el-card>
            <el-card>
                <template v-for="item in repos">
                    <el-link :href="'/'+props.owner+'/'+item.name" class="mx-1" size="large" type="primary">{{ item.name }}</el-link>
                    <br>
                    <el-text class="mx-1" size="small"> {{ item.description ?? 'the description is null '
                        }}</el-text><br>
                    <el-text class="mx-1" size="small">
                        {{ item.programmingLanguage ?? 'unkown' }}<el-divider direction="vertical" />
                        <el-icon>
                            <star />
                        </el-icon>{{ item.stargazers ?? 0 }}<el-divider direction="vertical" />
                        <el-icon> <knife-fork /> </el-icon>{{ item.forks ?? 0 }}<el-divider direction="vertical" />
                        {{ item.protocol ?? 'none' }}<el-divider direction="vertical" />
                        {{ item.updateTime }}
                    </el-text>
                    <el-divider />
                </template>
                <el-pagination layout="pager" v-model:current-page="currentPage" :page-size="30" :total="repoNum" />
            </el-card>
        </div>
    </div>
</template>


<style scoped>
#main {
    margin: 0 auto;
    max-width: 800px;
}
</style>