<template>
    <div class="transfer">
        <div class="box">
            <div class="left">
                <div class="title">
                    <label for="">
                        <input
                            type="checkbox"
                            name="check"
                            v-model="leftChecked"
                            @click="leftSelectAll($event)"/>
                        <span class="list-tile">列表1</span>
                    </label>
                </div>
                <div class="array-content">
                    <ul>
                        <li v-for="(item, index) of arrayLeft" :key="index">
                            <label class="checkbox">
                                <input
                                    type="checkbox"
                                    name="check"
                                    @click="leftSelect(index)"
                                    :checked="leftDatas.indexOf(index)>=0"/>
                                <span class="item-child">{{item.label}}</span>
                            </label>
                        </li>
                    </ul>
                </div>
            </div>
            <div class="center">
                <div
                    :class="[leftDatas.length>0?'checked':'', 'buttom']"
                    @click="shiftRight">
                    右移
                </div>
                <div
                    :class="[rightDatas.length>0?'checked':'', 'buttom']"
                    @click="shiftLeft">
                    左移
                </div>
            </div>
            <div class="right">
                <div class="title">
                    <label for="">
                        <input
                            type="checkbox"
                            name="check"
                            v-model="rightChecked"
                            @click="rightSelectAll($event)"/>
                        <span class="list-tile">列表2</span>
                    </label>
                </div>
                <div class="array-content">
                    <ul>
                        <li v-for="(item, index) of arrayRight" :key="index">
                            <label class="checkbox">
                                <input
                                    type="checkbox"
                                    name="check"
                                    :checked="rightDatas.indexOf(index)>=0"
                                    @click="rightSelect(index)"/>
                                <span class="item-child">{{item.label}}</span>
                            </label>
                        </li>
                    </ul>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'Transfer',
    props: {
        listLeft: Array,
        listRight: Array
    },
    data () {
        // // 列表1在加载时候获取的虚拟数据
        // const generateData = () => {
        //     const arraydata = []
        //     for (let i = 1; i <= 15; i++) {
        //         arraydata.push({
        //             key: i,
        //             label: `备选项${i}`
        //             // disabled: i % 4 === 0
        //         })
        //     }
        //     return arraydata
        // }
        return {
            // arrayLeft: generateData(),
            // arrayRight: [],
            arrayLeft: this.listLeft,
            arrayRight: this.listRight,
            data: [],
            leftDatas: [],
            rightDatas: [],
            leftChecked: false,
            rightChecked: false
        }
    },
    methods: {
        // 点击列表1全选时触发
        leftSelectAll ($event) {
            if ($event.currentTarget.checked === true) {
                // 全选先把数组清空，然后遍历数据，把所有行的index加入到数组中
                this.leftDatas = []
                for (let i = 0; i < this.arrayLeft.length; i++) {
                    this.leftDatas.push(i)
                }
                this.arrayLeft.forEach(element => {
                    this.data.push(element)
                })
            } else if (($event.currentTarget.checked === false)) {
                // 全不选,则把数组清空
                this.leftDatas = []
                // 把选择的数据内容清空
                this.data = []
            }
        },
        // 点击列表2全选时触发
        rightSelectAll ($event) {
            // 全选先把数组清空，然后遍历数据，把所有行的index加入到数组中
            if ($event.currentTarget.checked === true) {
                this.rightDatas = []
                for (let i = 0; i < this.arrayRight.length; i++) {
                    this.rightDatas.push(i)
                }
                this.arrayRight.forEach(element => {
                    this.data.push(element)
                })
            } else if ($event.currentTarget.checked === false) {
                // 全不选，则把数组清空
                this.rightDatas = []
                // 把选择的数据内容清空
                this.data = []
            }
        },
        // 点击列表1单个项时触发
        leftSelect (index) {
            // a为当前点击行index在leftDatas中的位置
            const a = this.leftDatas.indexOf(index)
            if (a >= 0) {
                // 存在的话，就从数组中删掉这个
                this.leftDatas.splice(a, 1)
                this.data.splice(index, 1)
            } else {
                // 不存在的话就加入到数组中
                this.leftDatas.push(index)
                this.data.push(this.arrayLeft[index])
            }
            // 判断全选按钮是否勾选
            if (this.leftDatas.length !== this.arrayLeft.length) {
                // 数组的长度不等于数据的长度，则全选按钮不勾选
                this.leftChecked = false
            } else {
                // 数组的长度等于数据的长度，全选按钮勾选
                this.leftChecked = true
            }
        },
        // 点击列表2单个项时触发
        rightSelect (index) {
            // b为当前点击行index在leftDatas中的位置
            const b = this.rightDatas.indexOf(index)
            if (b >= 0) {
                // 存在的话，就从数组中删除这个
                this.rightDatas.splice(b, 1)
                this.data.splice(index, 1)
            } else {
                // 不存在的话，就加入到数组中
                this.rightDatas.push(index)
                this.data.push(this.arrayRight[index])
            }
            // 判断全选按钮是否勾选
            if (this.arrayRight.length !== this.rightDatas.length) {
                // 数组的长度不等于数据的长度，则全选按钮不勾选
                this.rightChecked = false
            } else {
                // 数组的长度等于数据的长度。则全选按钮勾选
                this.rightChecked = true
            }
        },
        // 往列表2里面添加数据
        shiftRight () {
            if (this.data.length > 0) {
                // 列表2获取到数据后，经过去重跟重新排序
                this.arrayRight = [...new Set(this.arrayRight.concat(this.data))].sort((sel1, sel2) => {
                    if (sel1.key < sel2.key) {
                        return -1
                    } else if (sel1.key > sel2.key) {
                        return 1
                    } else {
                        return 0
                    }
                })
                // 删除已经从列表1移除的数据
                this.arrayRight.forEach(item => {
                    const index = this.arrayLeft.indexOf(item)
                    if (index >= 0) {
                        this.arrayLeft.splice(index, 1)
                    }
                })
                if (this.leftDatas.length > 0) {
                    this.resetArray()
                }
            }
        },
        // 移除列表2里面的数据
        shiftLeft () {
            if (this.data.length > 0) {
                // 列表1获取到数据后，经过去重跟重新排序
                this.arrayLeft = [...new Set(this.arrayLeft.concat(this.data))].sort((sel1, sel2) => {
                    if (sel1.key < sel2.key) {
                        return -1
                    } else if (sel1.key > sel2.key) {
                        return 1
                    } else {
                        return 0
                    }
                })
                // 删除已经从列表2移除的数据
                this.arrayLeft.forEach(item => {
                    const index = this.arrayRight.indexOf(item)
                    if (index >= 0) {
                        this.arrayRight.splice(index, 1)
                    }
                })
                if (this.rightDatas.length > 0) {
                    this.resetArray()
                }
            }
        },
        // 重置array/checked
        resetArray () {
            this.data = []
            this.leftDatas = []
            this.rightDatas = []
            this.leftChecked = false
            this.rightChecked = false
        }
    }
}
</script>

<style lang="scss" scoped>
.transfer {
    .box {
        display: flex;
        flex-direction: row;
        padding: 50px 20px;
        width: 800px;
        height: 300px;
        border: 1px solid #000;
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
        .left, .right {
            width: 200px;
            max-height: 100%;
            border: 1px solid #ebeef5;
            .title {
                margin: 0;
                padding-left: 15px;
                height: 40px;
                line-height: 40px;
                background: #f5f7fa;
                color: #000;
                border: 1px solid #ebeef5;
                box-sizing: border-box;
                .list-tile {
                    margin-left: 10px;
                }
            }
            .array-content {
                height: 246px;
                ul {
                    margin: 0;
                    padding: 6px 0 0;
                    height: 246px;
                    list-style-type: none;
                    overflow: auto;
                    box-sizing: border-box;
                    li {
                        height: 30px;
                        line-height: 30px;
                        padding-left: 15px;
                        margin-right: 30px;
                        .checkbox {
                            display: flex;
                            align-items: center;
                            color: #606266;
                            font-weight: 500;
                            font-size: 14px;
                            cursor: pointer;
                            white-space: nowrap;
                            &:hover {
                                color: #409eff;
                            }
                            .item-child {
                                width: 100%;
                                overflow: hidden;
                                text-overflow: ellipsis;
                                white-space: nowrap;
                                // display: inline-block;
                                box-sizing: border-box;
                                padding-left: 5px;
                                line-height: 30px;
                            }
                        }
                    }
                }
            }
        }
        .center {
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            margin: 0 15px;
            width: 100px;
            height: 300px;
            .buttom {
                margin: 0 0 10px;
                width: 55px;
                height: 40px;
                line-height: 40px;
                text-align: center;
                font-size: 12px;
                color: #fff;
                background: #a0cfff;
                border: 1px solid #fff;
                border-radius: 5px;
                cursor: pointer;
            }
            .checked {
                color: #fff;
                background: #409eff;
                border-color: #409eff;
            }
        }
    }
}
</style>
