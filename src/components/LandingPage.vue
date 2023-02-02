<template >
    <h1 v-if="loading">Loading......</h1>
    <div v-else class="selector">
        <button @click="changeSelected" class="btn">choose a movie</button>
        <div v-if="selected" class="selected">
            <h5 v-for="(item, index) in state" :key="index" @click="selectState(index)">
                <span class="item">
                    {{ index + 1 }} - {{ item.title }} : {{ item.release_date }}
                </span>
            </h5>
        </div>
    </div>
    <h1 v-if="loading2">Loading.....</h1>
    <section class="section" v-if="selectedState.length">
        
        <marquee behavior="" direction="left" class="marq"> {{ loaded }} </marquee>
        <select name="filter" @change="onChange($event)" id="filter">
            <option value="">All</option>
            <option v-for="item, index in gender" :value="item" :key="index">{{ item }}</option>
        </select>
        <TableVue @sorter="sorter" :fileterdState="fileterdState" :selectedState="selectedState" />
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
            loading2: false,
            sortTypeASC: true,
            error: false,
            genders: [],
            selected: false,
            errorMessage: "",
            filterValue: "",
            selectedState: [],
            starWarsSvg: require("../assets/star_wars_logo.svg"),
        };
    },
    mounted() {
        axios
            .get("https://swapi.dev/api/films")
            .then(({ data }) => {
                this.loading = false;
                this.state = data.results.map((title) => title);
                this.unmappedState = data.results.map((title) => title);
                console.log(this.state);
            })
            .catch((error) => console.log(error));
    },
    methods: {
        changeSelected() {
            this.selected = true;
        },
        selectState(index) {
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
                    console.log(this.selectedState);
                    // console.log(this.selectedState);
                    let gender1 = this.selectedState.map(character => character.gender);
                    this.gender = Array.from(new Set(gender1));
                    // const gender3 = Array.from(new Set(gender2.filter(Gender => Gender.gender)))
                    this.gender = this.gender.filter(Gender => Gender !== "none" && Gender !== "");
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
            // console.log(this.fileterdState);
        },
        sorter() {
            this.sortTypeASC = !this.sortTypeASC;
            this.sortTypeASC ? this.selectedState.sort((a, b) => a.name.localeCompare(b.name)) : this.selectedState.sort((a, b) => b.name.localeCompare(a.name));
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
    /* padding: 1%; */
    right: 0;
    top: 10%;

}

.marq {
    margin-top: 3%;
    margin-bottom: 5%;
    width: 100%;

}

</style>