<template >
    <div class="selector">
        <button @click="changeSelected" class="btn">choose a star wars movie</button>
        <div v-if="selected" class="selected">
            <h1 v-if="loading"><img :src="loadIcon" alt=""></h1>
            <h5 v-for="(item, index) in state" :key="index" @click="selectState(index)">
                <span class="item">
                    {{ index + 1 }} - {{ item.title }} : {{ item.release_date }}
                </span>
            </h5>
        </div>
    </div>
    <h1 v-if="loading2"><img :src="loadIcon" alt=""></h1>
    <section class="section" v-if="selectedState.length">

        <marquee behavior="" direction="left" class="marq"> {{ loaded }} </marquee>
        <div class="Heading">
            <h2 class="charater">Character List</h2>
            <select name="filter" @change="onChange($event)" id="filter">

                <option value="">Filter By</option>
                <option value="">All</option>
                <option v-for="item, index in gender" :key="index">{{ item }}</option>
            </select>
        </div>
        <TableVue @sorter="sorter" :height="height" :sortArrow="sortArrow" :feet="feet" :fileterdState="fileterdState"
            :selectedState="selectedState" />
    </section>
</template>
<script>
import axios from "axios";
import TableVue from "./Table.vue";

export default {
    name: "LandingPage",
    data() {
        return {
            unmappedState: [],
            state: [],
            loaded: [],
            fileterdState: [],
            loading: true,
            height: 0,
            loading2: false,
            sortTypeASC: true,
            error: false,
            genders: [],
            feet: [],
            selected: false,
            errorMessage: "",
            filterValue: "",
            selectedState: [],
            sortArrow: true,
            loadIcon: require("../assets/disastrousAltruisticCanary-size_restricted.gif"),
        };
    },
    mounted() {
        axios
            .get("https://swapi.dev/api/films")
            .then(({ data }) => {
                this.loading = false;
                this.state = data.results.map((title) => title);
                this.unmappedState = data.results.map((title) => title);
                // console.log(this.state);
            })
            .catch((error) => console.log(error));
    },
    methods: {
        changeSelected() {
            this.selected = true;

        },
        selectState(index) {
            this.selectedState = []

            this.loading2 = true;
            const loaded = this.unmappedState.filter((data, ind) => ind === index);
            this.loaded = (loaded[0].opening_crawl)
            let URLs = loaded[0].characters;
            function getAllData(URLs) {
                return Promise.all(URLs.map(fetchData));
            }
            function fetchData(URL) {
                return axios
                    .get(URL)
                    .then(function (response) {
                        return response.data;
                    })
                    .catch(function (error) {
                        return error.message;
                    });
            }
            getAllData(URLs)
                .then((resp) => {
                    this.loading2 = false;
                    localStorage.setItem("item", JSON.stringify(resp));
                    this.selectedState = JSON.parse(localStorage.getItem("item"));
                    // console.log(this.selectedState);
                    // console.log(this.selectedState);
                    let gender1 = this.selectedState.map(character => character.gender);
                    this.gender = Array.from(new Set(gender1));
                    // const gender3 = Array.from(new Set(gender2.filter(Gender => Gender.gender)))
                    this.gender = this.gender.filter(Gender => Gender !== "none" && Gender !== "");

                    let totalSum = 0
                    this.selectedState.map(State => {
                        const parsedValue = parseInt(State.height)
                        const current = parsedValue ? parsedValue : 0
                        totalSum += current
                    })
                    this.height = totalSum
                    this.toFeet(this.height)
                })
                .catch((e) => {
                    console.log(e);
                });
            this.selected = false;
        },
        onChange(event) {
            this.filterValue = event.target.value;
            let gender = this.selectedState.filter(value => value.gender == this.filterValue);
            this.fileterdState = gender;
            let totalSum = 0
            this.fileterdState.map(State => {
                const parsedValue = parseInt(State.height)
                const current = parsedValue ? parsedValue : 0
                totalSum += current
            })
            this.height = totalSum
            this.toFeet(this.height)
        },
        sorter() {

            this.sortTypeASC = !this.sortTypeASC;
            this.sortTypeASC ? this.selectedState.sort((a, b) => a.name.localeCompare(b.name)) : this.selectedState.sort((a, b) => b.name.localeCompare(a.name));
            this.sortArrow = !this.sortArrow
        },
        toFeet(n) {
            var realFeet = ((n * 0.393700) / 12);
            var feet = Math.floor(realFeet);
            var inches = Math.round(n / 2.54);
            this.feet = (feet + "ft/" + inches + 'in')
        }

    },
    components: { TableVue }
};
</script>
<style scoped >
.selector {
    display: inline-flex;
    flex-direction: column;
    color: #ff0;
    border: #ff0 2px solid;
    width: fit-content;
    height: fit-content;
    border-radius: 10px;
    text-align: start;
    margin-top: 3%;
}

.btn {
    text-align: start;
    width: 100%;
    color: #000;
    background: #ff0;
    border: none;
    font-size: 20px;
    outline: none;
}

.item {
    cursor: pointer;
}

.item:hover {
    color: #000;
    background: #ff0;
}

.selected {
    padding: 15px;
}

.section {
    position: relative;
}

#filter option {
    cursor: pointer;
}

#filter option:hover {
    color: #ff0;
    background: #000;
}

#filter {

    width: 17%;
    height: fit-content;
    border-radius: 10px;
    border: #ff0 2px solid;
    background: #ff0;
    color: #000;
    font-size: 1.5rem;

}


.Heading {
    width: 90%;
    display: inline-flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: baseline;
    margin-bottom: -4%;
}

.marq {
    margin-top: 3%;
    width: 100%;

}
</style>