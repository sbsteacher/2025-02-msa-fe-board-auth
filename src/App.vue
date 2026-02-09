<script setup>
import { useAuthenticationStore } from './stores/authentication';
import userService from './services/userService';

const authentication = useAuthenticationStore();

const doSignout = async () => {
  const result = await userService.signOut();
  console.log('result: ', result);
  console.log('typeof result.resultData: ', typeof result.resultData);
  if(result.resultData === 1) {
    authentication.signOut();
  } else {
    alert('로그아웃 실패!');
  }
}
</script>

<template>
<header>
  <router-link to="/">홈</router-link>
  <router-link to="/board/list">리스트</router-link>
  <div v-if="authentication.state.isSigned">
    {{ authentication.state.signedUser.nm }}님 환영합니다.
    <router-link to="/board/write">글쓰기</router-link>
    <a href="" @click.prevent="doSignout">로그아웃</a>
  </div>
  <div v-else>
    <router-link to="/signup">회원가입</router-link>
    <router-link to="/signin">로그인</router-link>
  </div>
  
</header>
<main>
  <router-view />
</main>
</template>

<style scoped>

</style>