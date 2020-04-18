<template>
    <div class="card mt-5">
        <h2 class="card-header">
            {{header}}
        </h2>
        <div class="m-3" v-if="numberAsteroids > 0 && showSummary">
            <p>showing {{numberAsteroids}} items</p>
            <p>{{closestObject}} has the shortest miss distance</p>
        </div>
        <div class="m-3">
            <a href="#" @click="showSummary = !showSummary">Show/Hide summary</a>
        </div>
        <table class="table table-stripped">
            <thead class="thead-light">
            <th>#</th>
            <th>Name</th>
            <th>Close Approach Date</th>
            <th>Miss Distance</th>
            <th>Remove</th>
            </thead>
            <tbody is="transition-group" name="neo-list">
            <tr v-for="(asteroid, index) in asteroids" :key="asteroid.neo_reference_id"
                :class="{highlight: isMissingData(asteroid)}">
                <td>{{index + 1}}</td>
                <td>{{asteroid.name}}</td>
                <td>{{getCloseApproachDate(asteroid)}}</td>
                <td>
                    <ul v-if="asteroid.close_approach_data.length > 0">
                        <li v-for="(value, key) in asteroid.close_approach_data[0].miss_distance" :key="value.id">
                            {{key}}: {{value}}
                        </li>
                    </ul>
                </td>
                <td>
                    <button @click="remove(index)" class="btn btn-warning">Remove</button>
                </td>
            </tr>
            </tbody>
        </table>
    </div>
</template>

<script>
    export default {
        props: ['asteroids', 'header'],
        name: "AsteroidGrid",
        data() {
            return {
                showSummary: true
            }
        },
        computed: {
            numberAsteroids() {
                return this.asteroids.length;
            },
            closestObject() {
                var neosHavingData = this.asteroids.filter(function (neo) {
                    return neo.close_approach_data.length;
                });
                var simpleNeos = neosHavingData.map(function (neo) {
                    return {name: neo.name, miles: neo.close_approach_data[0].miss_distance.miles}
                });
                var sortedNeos = simpleNeos.sort(function (a, b) {
                    return a.miles - b.miles;
                });

                return sortedNeos[0].name;
            }
        },
        methods: {
            getCloseApproachDate(asteroid) {
                if (asteroid.close_approach_data.length > 0) {
                    return asteroid.close_approach_data[0].close_approach_date;
                }
                return 'N/A';
            },
            isMissingData: function (asteroid) {
                return asteroid.close_approach_data.length == 0;
            },
            remove(index) {
                this.$emit('remove', index);
            }
        }
    }
</script>

<style scoped>
    .highlight {
        border: solid 3px red;
        color: red;
    }
</style>