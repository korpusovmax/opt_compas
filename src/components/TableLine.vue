<template xmlns="http://www.w3.org/1999/html">
    <div class="line" ref="line">
        <div class="line__column" :style="{width: headers[0].width+'px'}">
            <div class="line__menu_icon"></div>
            <p class="line__id">{{uid.id+1}}</p>
        </div>
        <div class="line__column" :style="{width: headers[1].width+'px'}">
            <div class="line__more_icon"></div>
            <div class="popup_delete">
                <div class="popup_delete__area">
                    <p class="popup_delete__text">Удалить</p>
                </div>
            </div>
        </div>
        <div class="line__column" :style="{width: headers[2].width+'px'}">
            <input type="text" class="line__input" :value="line.unit_name" @input="changeInput('unit_name', $event.target.value)">
        </div>
        <div class="line__column" :style="{width: headers[3].width+'px'}" @input="changeInput('coast', $event.target.value, true)">
            <input type="text" class="line__input" :value="line.coast">
        </div>
        <div class="line__column" :style="{width: headers[4].width+'px'}" @input="changeInput('count', $event.target.value, true)">
            <input type="text" class="line__input" :value="line.count">
        </div>
        <div class="line__column" :style="{width: headers[5].width+'px'}">
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
            console.log('delete')
            emit('deleteLine', key);
        }
        let cancelEvent = function(e) {
            console.log('cancel')
            popup.style.display = "none";
            popup.removeEventListener('mousedown', deleteEvent);
            document.removeEventListener('mousedown', cancelEvent);
        }
        this.element = document.getElementsByClassName('line');
        this.parent = document.getElementsByClassName('table__lines');
    },
    data() {
        return {
            index: 0,
            element: null,
            parent: null
        }
    },
    computed: {
        getIndex() {
            try {
                console.log(this.parent.children)
                console.log(Array.from(this.parent.children).indexOf(this.element))
                return Array.from(this.parent.children).indexOf(this.element);
            } catch (E) {
                console.log('a')
                return 0
            }

        }
    }
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
        padding-right: 10px;
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