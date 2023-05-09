<template>
    <div class="info_block">
        <div class="rect">
            <div class="label">
                <p class="label__text">Сумма:</p>
                <p class="label__value">{{summary.sum}} руб</p>
            </div>
            <div class="label">
                <p class="label__text">Кол-во:</p>
                <p class="label__value">{{summary.count}} шт</p>
            </div>
            <div class="label">
                <p class="label__text">Общий вес:</p>
                <p class="label__value">{{summary.weight}} кг</p>
            </div>
        </div>
        <div class="rect">
            <div class="label">
                <p class="label__value">Общая сумма:</p>
                <p class="label__value">{{summary.sum}} руб</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        summary: {
            type: Object,
            required: true
        }
    }
}
</script>

<style lang="scss" scoped>
.info_block {
    width: max-content;
    //height: 175px;
    height: 190px;
    //margin: -160px 15px 5px auto;
    margin: 15px 25px 5px auto;
    @media (min-width: 768px) {
        margin: -175px 15px 5px auto;
    }
}
.rect {
    padding: 15px;
    border-radius: 5px;
    border: solid 1px #eeeff1;
    background-color: #fbfcfd;

    &:first-child {
        width: 304px;
        height: 105px;
    }
    &:last-child {
        margin-top: 5px;
        width: 304px;
        height: 45px;
    }
}

.label {
    margin-bottom: 15px;
    display: flex;
    height: 15px;
    align-items: center;
    width: 100%;
    justify-content: space-between;
    &__valule {
        font-size: 14px;
        font-weight: normal;
        font-stretch: normal;
        font-style: normal;
        line-height: normal;
        letter-spacing: normal;
        text-align: right;
        color: #000;
    }
    &__text {
        font-size: 14px;
        font-weight: normal;
        font-stretch: normal;
        font-style: normal;
        line-height: normal;
        letter-spacing: normal;
        color: #8f8f8f;
    }
}
</style>