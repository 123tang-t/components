<template>
  <div class="home">
    <!-- <Pagea/>
    <Pageb/> -->
    <!-- <Transfer/> -->
    <div class="text" @click="jump">
        跳转
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
// import Pagea from '@/components/a.vue'
// import Pageb from '@/components/b.vue'
// import Transfer from '@/components/transfer.vue'

export default {
    name: 'Home',
    components: {
        // Pagea,
        // // Pageb,
        // Transfer
    },
    methods: {
        jump () {
            this.$router.push({
                path: '/about'
            })
        }
    }
}
</script>

<style lang="scss" scoped>
.home {
    .text {
        height: 50px;
        width: 50px;
        line-height: 30px;
        text-align: center;
    }
}
</style>
