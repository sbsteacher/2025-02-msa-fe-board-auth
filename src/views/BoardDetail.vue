<script setup>
import { onMounted, reactive } from 'vue';
import { useRoute, useRouter } from 'vue-router';
import { useAuthenticationStore } from '@/stores/authentication';
import boardService from '@/services/boardService';

const route = useRoute();
const router = useRouter();

const authentication = useAuthenticationStore();

const state = reactive({
    data: {}
});

onMounted(async () => {
    const id = route.params.id;
    console.log('id: ', id);
    const result = await boardService.getBoard(id);
    state.data = result.resultData;
});

const doDelete = async () => {
    if(!confirm('삭제하시겠습니까?')) { return; }
    const params = {
        id: route.params.id
    }
    const result = await boardService.delBoard(params);
    if(result.resultData) {
        router.push('/board/list');
    } else {
        alert(result.resultMessage);
    }
}

const goToMod = () => {    
    router.push({
        path: '/board/write',
        state: {
            data: {
                id: route.params.id
                , title: state.data.title
                , contents: state.data.contents
            }
        }
    });
}
</script>

<template>
<h3>게시판 디테일</h3>
<div>{{ state.data.title }}</div>
<div>{{ state.data.nm }}</div>
<div>{{ state.data.createdAt }}</div>
<div>{{ state.data.contents }}</div>
<div v-show="authentication.state.signedUser?.signedUserId === state.data.userId">
    <button @click="doDelete">삭제</button>
    <button @click="goToMod">수정</button>
</div>
</template>

<style scoped>

</style>