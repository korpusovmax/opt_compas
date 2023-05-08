<template>
    <div class="main">
        <SideBar></SideBar>
        <div class="main__content">
            <h1>Проведение ТО и мелкий ремонт</h1>
            <div class="tabs">
                <p class="tabs__item">Общее</p>
                <p class="tabs__item tabs__item--active">Товар списания</p>
                <p class="tabs__item">Доп. расходы</p>
            </div>

            <TableView></TableView>
        </div>
    </div>
</template>

<script>
import SideBar from "@/components/SideBar.vue";
import TableView from "@/components/TableView.vue";
export default {
    components: {
        SideBar, TableView
    },
}
</script>

<style lang="scss">
@font-face {
    font-family: 'Myriad Pro';
    src: local('Myriad Pro Regular'), local('Myriad-Pro-Regular'),
    url('./assets/fonts/MyriadPro-Regular.woff2') format('woff2'),
    url('./assets/fonts/MyriadPro-Regular.woff') format('woff'),
    url('./assets/fonts/MyriadPro-Regular.ttf') format('truetype');
    font-weight: 400;
    font-style: normal;
}

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Myriad Pro', sans-serif;
    user-select: none;
}
.main {
    height: 100%;
    min-height: 100vh;
    display: flex;
    //padding: 0 25px 0 0;
    background-color: #fbfcfd;
    &__content {
        padding: 20px 0 0 25px;
        overflow:hidden;
        max-width: 100%;
        width: 100%;
    }
}
.tabs {
    display: flex;
    margin-bottom: 25px;
    &__item {
        display: inline-block;
        margin-right: 22px;
        font-size: 16px;
        font-weight: 600;
        font-stretch: normal;
        font-style: normal;
        line-height: normal;
        letter-spacing: normal;
        color: #1253a2;
        &--active {
            color: #000000;
        }
        &:hover {
            opacity: 70%;
        }
    }
}
h1 {
    margin-bottom: 25px;
    height: 31px;
    font-size: 30px;
    font-weight: normal;
    font-stretch: normal;
    font-style: normal;
    line-height: normal;
    letter-spacing: normal;
    color: #000;
}

</style>