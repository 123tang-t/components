<template>
  <div class="home">
    <Transfer
        v-model="value"
        :dataSource="dataSource"/>
  </div>
</template>

<script>
import Transfer from '@/components/transfer/transfer.vue'

export default {
    name: 'Home',
    components: {
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
            value: [1, 4]
        }
    }
}
</script>

<style lang="scss" scoped>
</style>
