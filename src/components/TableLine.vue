<template xmlns="http://www.w3.org/1999/html">
    <div class="line" ref="line">
        <div v-show="!mobile" class="line__column" :style="{width: headers[0].width+'px'}">
            <div class="line__menu_icon"></div>
            <p class="line__id">{{uid.id+1}}</p>
        </div>
        <div class="line__column" :style="{width: headers[1].width+'px'}">
            <p class="line__label">{{ headers[1].name }}</p>
            <div class="line__more_icon"></div>
            <div class="popup_delete">
                <div class="popup_delete__area">
                    <p class="popup_delete__text">Удалить</p>
                </div>
            </div>
        </div>
        <div class="line__column" v-show="headers[2].active" :style="{width: mobile ? '100%' : headers[2].width+'px'}">
            <p class="line__label">{{ headers[2].name }}</p>
            <input type="text" class="line__input" :value="line.unit_name" @input="changeInput('unit_name', $event.target.value)">
        </div>
        <div class="line__column" v-show="headers[3].active" :style="{width: mobile ? '100%' : headers[3].width+'px'}" @input="changeInput('coast', $event.target.value, true)">
            <p class="line__label">{{ headers[3].name }}</p>
            <input type="text" class="line__input" :value="line.coast">
        </div>
        <div class="line__column" v-show="headers[4].active" :style="{width: mobile ? '100%' : headers[4].width+'px'}" @input="changeInput('count', $event.target.value, true)">
            <p class="line__label">{{ headers[4].name }}</p>
            <input type="text" class="line__input" :value="line.count">
        </div>
        <div class="line__column" v-show="headers[5].active" :style="{width: mobile ? '100%' : headers[5].width+'px'}">
            <p class="line__label">{{ headers[5].name }}</p>
            <p class="line__text">{{ line.sum }}</p>
        </div>
    </div>
</template>

<script>

import {toRaw} from "vue";

export default {
    methods: {
        changeInput(name, value, number=false) {
            if (number) {
                value = parseInt(value);
                if (value || value === 0) this.$emit('changeInput', name, value, this.line.key);
            } else {
                this.$emit('changeInput', name, value, this.line.key);
            }
        }
    },
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
        },
        mobile: {
            type: Boolean,
            required: true
        }
    },
    mounted() {
        //add delete popup events
        const key = this.line.key;
        const emit = this.$emit;
        const more_icon = document.getElementsByClassName('line__more_icon')[this.$props.uid.id];
        const popup = document.getElementsByClassName('popup_delete')[this.$props.uid.id];
        more_icon.addEventListener('click', function () {
            popup.style.display = "block";
            popup.addEventListener('mousedown', deleteEvent);
            document.addEventListener('mousedown', cancelEvent);
        })
        let deleteEvent = function (e) {
            emit('deleteLine', key);
        }
        let cancelEvent = function(e) {
            popup.style.display = "none";
            popup.removeEventListener('mousedown', deleteEvent);
            document.removeEventListener('mousedown', cancelEvent);
        }
    },
    data() {
        return {
            index: 0
        }
    }
}
</script>

<style lang="scss" scoped>
.line {
    user-select: none;
    flex-wrap: wrap;
    display: flex;
    flex-direction: column;
    margin-top: 5px;
    @media (min-width: 768px) {
        flex-direction: row;
        flex-wrap: nowrap;
        height: 45px;
        margin: 0;
        align-items: center;
        width: max-content;
    }
    @media (max-width: 768px) {
        padding: 0 0 25px 0;
        border-radius: 10px;
        box-shadow: 0 5px 20px 0 rgba(0, 0, 0, 0.07);
        border: solid 1px #eeeff1;
        background-color: #fff;
    }

    &:last-child {
        margin-bottom: 0;
    }

    &__column {
        padding-left: 10px;
        display: flex;
        align-items: center;

        overflow:hidden;
        white-space:nowrap;
        text-overflow: ellipsis;
        padding-right: 10px;
        @media (max-width: 768px) {
            margin-top: 15px;
            flex-direction: column;
            align-items: start;
        }
    }
    &__column:first-child {
        padding-left: 15px;
    }
    &__column:last-child {
        //padding-right: 15px;
    }

    &__menu_icon {
        margin-right: 5px;
        background-image: url(../assets/burger_menu.png);
        background-repeat: no-repeat;
        background-size: contain;
        width: 11px;
        height: 12px;
    }
    &__id, &__text {
        font-size: 16px;
        font-weight: normal;
        font-stretch: normal;
        font-style: normal;
        line-height: normal;
        letter-spacing: normal;
        color: #000;
    }
    &__more_icon {
        background-image: url(../assets/more.png);
        background-repeat: no-repeat;
        background-size: contain;
        width: 13px;
        height: 13px;
    }
    &__input {
        width: 100%;
        height: 35px;
        margin-right: 10px;
        padding: 10px 0 9px 15px;
        border-radius: 5px;
        border: solid 1px #ccc;
        background-color: #fff;
    }
    &:last-child .line__input{
        margin-right: 0;
    }
    &__label {
        @media (min-width: 768px) {
            display: none;
        }
        margin-bottom: 5px;
        font-size: 10px;
        font-weight: normal;
        font-stretch: normal;
        font-style: normal;
        line-height: normal;
        letter-spacing: normal;
        color: #8f8f8f;
    }
}

.popup_delete {
    position: absolute;
    //right: 40px;
    display: none;
    width: 179px;
    height: 29px;
    padding: 7px 9.9px 7px 10px;
    border-radius: 5px;
    box-shadow: 0 0 3px 0 #000, inset 0 1px 2px 0 rgba(255, 255, 255, 0.5);
    background-color: #fff;
    &__area {
        display: flex;
        align-items: center;
    }
    &__text {
        font-size: 14px;
        font-weight: normal;
        font-stretch: normal;
        font-style: normal;
        line-height: normal;
        letter-spacing: normal;
        color: #ae0a0a;
    }
}
</style>