<template>
<div>
    <Header></Header>
    <router-view />
</div>
</template>

<script>
import Header from './components/Header'

export default {
  name: 'main-app',
  components: {
    'Header': Header,
  }
}
</script>