<script setup>
import { reactive, onMounted } from 'vue';
import boardService from '@/services/boardService';

const state = reactive({
    list: [],
    searchText: '',
    size: 50,
    currentPage: 1,
    maxPage: 0
});

const getBoardMaxPage = async () => {
    const params = { size: state.size };
    if(state.searchText) {
        params.search_text = state.searchText;
    }
    const result = await boardService.getBoardMaxPage( params );
    state.maxPage = result.resultData;
}

onMounted(async () => {
    getBoardMaxPage();

    const params = {
        page: state.currentPage
        , size: state.size
    };
    if(state.searchText) {
        params.search_text = state.searchText;
    }
    const result = await boardService.getBoardList( params );
    state.list = result.resultData;
});

</script>

<template>
<h3>게시판 리스트</h3>
<div><input type="search" v-model="state.searchText"><button>검색</button></div>
<div v-if="state.list.length === 0">게시글이 없습니다.</div>
<div v-else>
    <table>
        <thead>
            <tr>
                <th>no</th>
                <th>title</th>
                <th>writer</th>
                <th>created at</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="item in state.list" :key="item.id">
                <td>{{ item.id }}</td>
                <td>{{ item.title }}</td>
                <td>{{ item.nm }}</td>
                <td>{{ item.createdAt }}</td>
            </tr>
        </tbody>
    </table>
    <div>
        <span class="page" :class="item == state.currentPage ? 'selected': ''" v-for="item in state.maxPage" :key="item">{{ item }}</span>
    </div>
</div>

</template>

<style scoped>
table { border-collapse: collapse; }
table td, table th { border: 1px solid #eee; padding: 10px; }
table tbody tr:hover { background-color: aliceblue; cursor: pointer;}
.page:not(:first-child) { margin-left: 8px; }
.selected { color: red; font-weight: bold; }
</style>