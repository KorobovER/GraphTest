<script>
import indicators from '../../data/data.js';
import Indicator from './Indicator.vue';

export default {
    components: {
        Indicator
    },
    data() {
        return {
            indicators
        }
    }
}

</script>

<template>
    <div class="table">
        <div class="row">
            <div class="cell">Показатель</div>
            <div class="cell this-day">Текущий день</div>
            <div class="cell">Вчера</div>
            <div class="cell">Этот день недели</div>
        </div>
        <div v-for="indicator in indicators" :key="indicator.id">
            <Indicator :indicator="indicator" />
        </div>

    </div>
</template>

<style scoped>
.table {
    display: flex;
    flex-direction: column;
    width: 1000px;
    height: fit-content;
    background-color: white;
    border: 1px solid gray;
    margin: 50px 0 50px 0;
}

.row {
    display: flex;
    width: 100%;
}

.cell:first-child {
    width: 500px;
}

.cell {
    width: 160px;
    text-align: center;
    padding: 15px;
    background-color: rgb(225, 225, 225);
    margin: 5px;
}
.this-day {
  background-color: rgb(127, 238, 255);
}
</style>
