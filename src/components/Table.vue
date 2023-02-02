<template>
    <div class="overflow">
        <table class="table">
            <thead class="thead">
                <tr @click="$emit('sorter')" class="trHead">
                    <th>Name
                        <b  v-if="sortArrow === true"><i class="fa-solid fa-arrow-down-long"></i>
                        </b>
                        <b v-else-if="sortArrow === false">
                            <i else class="fa-solid fa-arrow-up-long"></i>
                        </b>
                    </th>
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
                <tr class="trBody">

                    <td span="2"> Total </td>

                    <td>{{ height }} cm ({{ feet }})</td>
                </tr>

            </tbody>
            <tbody v-else-if="selectedState.length">
                <tr class="trBody" v-for="(item, index) in selectedState" :key="index">
                    <td>{{ item.name }}</td>
                    <td>{{ item.gender }}</td>
                    <td>{{ item.height }} </td>

                </tr>
                <tr class="trBody">

                    <td span="2"> Total </td>

                    <td>{{ height }} cm ({{ feet }})</td>
                </tr>
            </tbody>
        </table>
    </div>
</template>
<script>
export default {
    name: 'TableVue',
    props: {
        fileterdState: Array,
        selectedState: Array,
        height: Number,
        feet: Array,
        sortArrow: Boolean,
    },
    sorter() {
        this.$emit('sorter')
    }
}
</script>
<style scoped>
.overflow {
    overflow-x: scroll;
}


.table,
tr,
td {
    border: #ff0 2px solid;
    border-radius: 150px;
    color: #ff0;
    font-size: 14px;
    margin-top: 1%;
}

.trHead {
    background-color: #ff0;
    color: #000;
    font-size: 20px;
}

.trBody {
    font-size: 17px
}
</style>