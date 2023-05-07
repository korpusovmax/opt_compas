<template>
    <div class="line">
        <div class="line__column" :style="{width: headers[0].width+'px'}">
            <div class="line__menu_icon"></div>
            <p class="line__id">{{uid.id+1}}</p>
        </div>
        <div class="line__column" :style="{width: headers[1].width+'px'}">
            {{ line.unit_name }}
        </div>
        <div class="line__column" :style="{width: headers[2].width+'px'}">
            {{ line.coast }}
        </div>
        <div class="line__column" :style="{width: headers[3].width+'px'}">
            {{ line.count }}
        </div>
    </div>
</template>

<script>

export default {
    props: {
        uid: {
            type: Object,
            required: true,
        },
        line: {
            type: Object,
            required: true
        },
        headers: {
            type: Array,
            required: true
        }
    },
}
</script>

<style lang="scss" scoped>
.line {
    user-select: none;
    height: 45px;
    display: flex;
    align-items: center;
    width: max-content;
    &__column {
        padding-left: 10px;
        display: flex;
        align-items: center;

        overflow:hidden;
        white-space:nowrap;
        text-overflow: ellipsis;
    }
    &__column:first-child {
        padding-left: 15px;
    }
    &__column:last-child {
        padding-right: 15px;
    }
    &__menu_icon {
        margin-right: 5px;
        background-image: url(../assets/burger_menu.png);
        background-repeat: no-repeat;
        background-size: contain;
        width: 11px;
        height: 12px;
    }
    &__id {
        font-size: 16px;
        font-weight: normal;
        font-stretch: normal;
        font-style: normal;
        line-height: normal;
        letter-spacing: normal;
        color: #000;
    }
}
</style>