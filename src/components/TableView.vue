<template>
    <div>
        <div class="block">
            <div class="block__btn" @click="addNewLine()"><div class="block__icon"></div><p class="block__text">Добавить строку</p></div>
        </div>
        <div class="table_block">
            <div class="table_block__settings_icon"></div>
            <div class="popup">
                <div class="popup__area">
                    <p class="popup__text">Отображение столбцов</p><div class="popup__icon"></div>
                </div>
                <div class="popup__items">
                    <div v-for="(header, idx) in headers">
                        <CheckBoxItem v-if="idx>1" :header="header" @changeActiveState="changeColumnVisible(idx)"></CheckBoxItem>
                    </div>
                </div>
            </div>
            <div class="table">
                <TableHeaders :mobile="mobile" :headers="headers" @updateWidth="changeWidth"></TableHeaders>
                <div class="table__lines sortable">
                    <div class="table__line_block" draggable="true" v-for="(line, idx) in lines">
                        <TableLine :headers="headers" :uid="lines[idx]" :line="table[idx]" :mobile="mobile" @changeInput="changeInput" @deleteLine="deleteLine"></TableLine>
                    </div>
                </div>
            </div>
            <TableInformation v-if="!mobile" :summary="getSummary()"></TableInformation>
        </div>
        <TableInformation v-if="mobile" :summary="getSummary()"></TableInformation>
    </div>
</template>

<script>
import TableLine from "@/components/TableLine.vue";
import TableHeaders from "@/components/TableHeaders.vue";
import {toRaw} from "vue";
import TableInformation from "@/components/TableInformation.vue";
import sortable from "../assets/lib/sortable";
import CheckBoxItem from "@/components/CheckBoxItem.vue";


export default {
    components: {
        CheckBoxItem,
        TableInformation,
        TableHeaders,
        TableLine
    },
    data() {
        return {
            mobile: false,
            lines: [
                {id: 0},
                {id: 1},
                {id: 2}
            ],
            table: [
                {key: 0, unit_name: 'Мраморный щебень фр. 2-5 мм', coast: 1231, count: 1, product_name: 'Мраморный щебень', sum: 1231},
                {key: 1, unit_name: 'Мраморный щебень фр. 2-5 мм, 25 кг', coast: 1100, count: 5, product_name: 'Мраморный щебень', sum: 5500},
                {key: 1, unit_name: 'Мраморный щебень, 6 кг', coast: 1150, count: 2, product_name: 'Мраморный щебень', sum: 2300},
            ],
            headers: [
                {name: 'Номер', width: 60, active: true},
                {name: 'Действие', width: 60, active: true},
                {name: 'Наименование единицы', width: 230, active: true},
                {name: 'Цена', width: 80, active: true},
                {name: 'Кол-во', width: 80, active: true},
                {name: 'Итого', width: 60, active: true}
            ],
            table_lines_element: null,
        }
    },
    methods: {
        onResize() {
            this.mobile = window.innerWidth <= 768;
        },
        changeInput(name, value, key) {
            for (let i = 0; i < this.table.length; i++) {
                if (this.table[i].key == key) {
                    this.table[i][name] = value;
                    this.table[i]['sum'] = this.table[i].count*this.table[i].coast;
                }
            }
        },
        changeWidth(id, width) {
            this.headers[id].width = width;
        },
        getSummary() {
            let result = {sum: 0, count: 0, weight: 0};
            for (let i = 0; i < this.table.length; i++) {
                let weight_matches = this.table[i].unit_name.match(/[0-9]*\s?(кг)/);
                if (weight_matches) result.weight += parseInt(weight_matches[0].match(/[0-9]*/)[0])*this.table[i].count;
                result.sum += this.table[i].sum;
                result.count += this.table[i].count;
            }
            return result;
        },
        addNewLine() {
            let id = this.lines.length;
            this.table.push({key: id, unit_name: 'test', coast: 666, count: 0, product_name: 'test', sum: 0});
            this.lines.push({id: id});
            console.log(this.lines)
            this.$nextTick(() => {
                this.destroySortable();
                this.initSortable();
            });

        },
        deleteLine(id) {
            this.table.splice(id, 1);
            this.lines.splice(id, 1);
            this.$nextTick(() => {
                this.destroySortable();
                this.initSortable();
            });
        },
        changeColumnVisible(id) {
            this.headers[id].active = !this.headers[id].active;
        },
        initSortable(table_lines=null, create=false) {
            if (!table_lines) {
                let element_lines = this.table_lines_element.getElementsByClassName('line__id');
                for (let i = 0; i < element_lines.length; i++) element_lines[i].innerHTML = i+1;
            } else {
                this.table_lines_element = table_lines;
                let element_lines = this.table_lines_element.getElementsByClassName('line__id');
                for (let i = 0; i < element_lines.length; i++) element_lines[i].innerHTML = i+1;
            }

            let lines = this.lines;
            let copy = [];
            for (let i = 0; i < lines.length; i++) copy.push({id: i});

            let table_sort = sortable('.table__lines', {
                placeholderClass: 'table__place_holder'
            });

            let sortUpdate = function(e) {
                let element_lines = table_lines.getElementsByClassName('line__id');
                for (let i = 0; i < element_lines.length; i++) element_lines[i].innerHTML = i+1;

            };
            if (create) table_sort[0].addEventListener('sortupdate', sortUpdate);
        },
        destroySortable() {
            sortable('.table__lines', 'destroy');
        },
    },
    mounted() {
        window.addEventListener('resize', this.onResize);
        this.onResize();

        this.initSortable(document.getElementsByClassName('table__lines')[0], true);

        //add settings popup events
        const settings_icon = document.getElementsByClassName('table_block__settings_icon')[0];
        const popup = document.getElementsByClassName('popup')[0];
        const popup_items = document.getElementsByClassName('popup__items')[0];
        const nextTick = this.$nextTick;

        const clickEvent = function (e) {
            settings_icon.removeEventListener('mousedown', clickEvent);
            document.addEventListener('mousedown', cancelPopupEvent);
            e.stopPropagation();
            popup.style.display = "flex";
        };

        function cancelPopupEvent(e) {
            e.stopPropagation();
            popup.style.display = "none";
            popup_items.style.display = "none";
            document.removeEventListener('mousedown', cancelPopupEvent);
            settings_icon.addEventListener('mousedown', clickEvent)
        }
        popup.addEventListener('mousedown', function (e) {
            popup_items.style.display = "block";
            e.stopPropagation();
        });
        settings_icon.addEventListener('mousedown', clickEvent);
    }
}
</script>

<style lang="scss">
.block {
    height: 75px;
    margin-bottom: 25px;
    @media (max-width: 768px) {
        margin-bottom: 20px;
    }
    margin-right: 25px;
    padding: 20px 0 20px 25px;
    border-radius: 10px;
    box-shadow: 0 5px 20px 0 rgba(0, 0, 0, 0.07);
    border: solid 1px #eeeff1;
    background-color: #fff;
    &__btn {
        width: 146px;
        height: 35px;
        padding: 10px 15px 10px 10px;
        display: flex;
        align-items: center;
        border-radius: 5px;
        background-color: #2f8cff;
    }
    &__icon {
        background-image: url('../assets/plus.png');
        background-repeat: no-repeat;
        background-size: contain;
        width: 11px;
        height: 11px;
        margin-right: 7px;
    }
    &__text {
        font-size: 14px;
        font-weight: normal;
        font-stretch: normal;
        font-style: normal;
        line-height: normal;
        letter-spacing: normal;
        color: #fff;
    }
}
.popup {
    position: absolute;
    right: 40px;
    display: none;
    flex-direction: column;
    width: 179px;

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
        color: #161616;
    }
    &__icon {
        margin-left: 11px;
        margin-top: 1px;
        width: 4.1px;
        height: 9px;
        background-size: contain;
        background-repeat: no-repeat;
        background-image: url('../assets/arrow.png');
    }

    &__items {
        display: none;
    }
}

.table_block {
    margin-right: 25px;
    margin-bottom: 25px;
    //width: 100%;
    max-width: 100%;
    @media (min-width: 768px) {
        padding: 9px 0 0 0;
        border-radius: 10px;
        box-shadow: 0 5px 20px 0 rgba(0, 0, 0, 0.07);
        border: solid 1px #eeeff1;
        background-color: #fff;
    }


    &__settings_icon {
        margin: 0 15px 8px auto;
        @media (max-width: 768px) {
            display: none;
        }
        width: 15px;
        height: 15px;
        background-repeat: no-repeat;
        background-image: url('../assets/settings.png');
        background-size: contain;
        background-position: right center;
        &:hover {
            background-image: url('../assets/settings_active.png');
        }
    }
}
.table {
    //justify-items: end;

    @media (min-width: 768px) {
        display: flex;
        flex-direction: column;
        flex-wrap: nowrap;
        overflow-x: auto;
        padding-bottom: 180px;
    }
}
.table__place_holder {
    user-select: none;
    height: 45px;
    width: 100%;
    align-self: end;
    position: sticky;
    border-radius: 5px;
    border: dashed 2px #a6b7d4;
    background-color: #fbfcfd;
}
</style>