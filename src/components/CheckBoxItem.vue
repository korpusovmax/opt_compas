<template>
    <div class="item" @click="changeActiveState">
        <div class="item__checkbox" :class="{active: header.active}"></div>
        <p class="item__text">{{header.name}}</p>
    </div>
</template>

<script>
export default {
    props: {
        header: {
            type: Object,
            required: true
        }
    },
    methods: {
        changeActiveState() {
            this.$emit('changeActiveState')
        }
    }
}
</script>

<style lang="scss" scoped>
.item {
    display: flex;
    margin-top: 14px;
    &__checkbox {
        width: 13px;
        height: 13px;
        margin: 0 4px 0 0;
        border-radius: 3px;
        border: solid 1px #a6b7d4;
    }
    &__text {
        margin-left: 4px;
        font-size: 14px;
        font-weight: normal;
        font-stretch: normal;
        font-style: normal;
        line-height: normal;
        letter-spacing: normal;
        color: #161616;
    }
}
.active {
    padding: 4px 2px 3.1px;
    border-radius: 3px;
    background-image: url("../assets/mark.png");
    background-size: 9px 6px;
    background-repeat: no-repeat;
    background-position: center center;
    background-color: #3261ff;
}
</style>