<template>
  <div class="home">
    <!-- <Pagea/>
    <Pageb/> -->
    <Transfer
        v-model="value"
        :dataSource="dataSource"/>
        <div v-for="item of value" :key="item">{{item}}</div>
  </div>
</template>

<script>
// import Pagea from '@/components/a.vue'
// import Pageb from '@/components/b.vue'
import Transfer from '@/components/transfer/transfer.vue'

export default {
    name: 'Home',
    components: {
        // Pagea,
        // // Pageb,
        Transfer
    },
    data () {
        // 生成虚拟数据的数据源
        const generateData = () => {
            const arraydata = []
            for (let i = 1; i <= 15; i++) {
                arraydata.push({
                    key: i,
                    label: `备选项${i}`
                    // disabled: i % 4 === 0
                })
            }
            return arraydata
        }
        return {
            dataSource: generateData(),
            value: []
        }
    }
}
</script>

<style lang="scss" scoped>
</style>
