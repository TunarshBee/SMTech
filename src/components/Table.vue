<template>
<div class="overflow">
    <h2 class="character">Character List</h2>
    <table class="table">
        <thead class="thead">
            <tr @click="sorter" class="trHead">
                <th>Name</th>
                <th>Gender</th>
                <th>Height</th>
            </tr>
        </thead>
        <tbody v-if="fileterdState.length">
            <tr class="trBody" v-for="(item, index) in fileterdState" :key="index">
                <td>{{ item.name }}</td>
                <td>{{ item.gender }}</td>
                <td>{{ item.height }}</td>
            </tr>
        </tbody>
        <tbody v-else-if="selectedState.length">
            <tr class="trBody" v-for="(item, index) in selectedState" :key="index">
                <td>{{ item.name }}</td>
                <td>{{ item.gender }}</td>
                <td>{{ item.height }}</td>
            </tr>
        </tbody>
    </table>
</div>
</template>
<script>
export default {
    name:'TableVue',
    props: {
        fileterdState: Array,
        selectedState: Array
    },
    sorter(){
        this.$emit('sorter')
    }
}
</script>
<style scoped>
.overflow {
    overflow-x: scroll;
}
.table, tr,td{
    border: #ff0 2px solid;
    color: #ff0;
    font-size: 14px;
    margin-top: 5%;
}
.trHead{
    background-color: #ff0;
    color: #000;
    font-size: 20px;
}
.trBody{
    font-size: 17px
}
</style>