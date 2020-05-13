<template>
    <div class="paging">
        <button v-if="!(current <= count)" @click="goFirst()"><<</button>
        <button v-if="!(current <= count)" @click="prevGroup()"><</button>
        <button v-for="(idx,key) in pagingGroup" :key="key" :class="idx==setPaging ? 'active':''" @click="movePaging(idx)">{{idx}}</button>
        <button v-if="current + count < max+1" @click="nextGroup()">></button>
        <button v-if="current + count < max+1" @click="goLast()">>></button>
    </div>
</template>

<script>
export default {
    name:"paging",
    props:{
        maxPaging : Number,
        setPaging: Number
    },

    created() {
        this.updateList();
    },

    data() {
        return {
            pagingGroup : [],
            count : 5,
            max : 1,
            current : 1
        }
    },

    watch: {
        setPaging() {
            this.current = this.setPaging;
            this.updateList();
        }
    },
    methods : {
        updateList() {
            this.max = this.maxPaging;
            this.current = this.setPaging;
            this.pagingGroup = [];

            const temp = this.current % this.count !== 0 ? 0 : -1;
            const startNumber = (this.count * ( parseInt(this.current / this.count) + temp) ) + 1;
            
            for(let i = startNumber; i < (startNumber+this.count); ++i){
                if(i <= this.max) {
                    this.pagingGroup.push(i);
                }
            }
        },
        movePaging(idx) {
            this.$emit('movePaging', idx);
        },
        nextGroup() {
            this.$emit('movePaging', this.pagingGroup[this.pagingGroup.length-1] + 1);
        },
        prevGroup() {
            this.$emit('movePaging', (this.count * ( (this.pagingGroup[0] - 1) / this.count - 1) + 1));
        },
        goFirst() {
            this.$emit('movePaging', 1);
        },
        goLast() {
            this.$emit('movePaging', this.maxPaging);
        }
    }
}
</script>

<style scope>
    .paging button {
        margin:0 3px;
    }
    .paging button.active {
        background:#000;
        color:#fff;
        border-color:#000;
    }
</style>