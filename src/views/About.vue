<template>
    <div class="about">
        <TransferTwo
            :listLeft="arrayLeft"
            :listRight="arrayRight"/>
    </div>
</template>

<script>
import TransferTwo from '@/components/transferTwo.vue'
export default {
    name: 'about',
    components: {
        TransferTwo
    },
    data () {
        // 列表1在加载时候获取的虚拟数据
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
            arrayLeft: generateData(),
            arrayRight: []
        }
    }
}
</script>
