<script setup>
  
</script>

<template>
    <div class="table">
        <div class="row">
            <div class="cell">Показатель</div>
            <div class="cell">Текущий день</div>
            <div class="cell">Вчера</div>
            <div class="cell">Этот день недели</div>
        </div>
    </div>
</template>

<style>
    .table{
        display: flex;
        justify-content: center;
        width: 1000px;
        height: 700px;
        background-color: white;
        border:1px solid gray;
        margin: 50px 0 50px 0;
    }
    .row{
        display: flex;
        width: 100%;
        gap: 10px;
    }
    .cell{ 
        width: 25%;
        text-align: center;
        padding: 15px;
        height: fit-content;
        background-color: rgb(225, 225, 225);
    }
</style>
