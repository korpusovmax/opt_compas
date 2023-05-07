<template>
    <div class="table_block">
        <div class="table_block__settings_icon"></div>
        <div class="table">
            <TableHeaders :headers="headers" @updateWidth="changeWidth"></TableHeaders>
            <div class="table__lines sortable">
                <div class="table__line_block" v-for="(line, idx) in lines">
                    <TableLine :headers="headers" :uid="lines[idx]" :line="table[idx]"></TableLine>
                </div>
            </div>
        </div>
        <TableInformation></TableInformation>
    </div>
</template>

<script>
import TableLine from "@/components/TableLine.vue";
import TableHeaders from "@/components/TableHeaders.vue";
import {toRaw} from "vue";
import TableInformation from "@/assets/TableInformation.vue";
import sortable from "../assets/lib/sortable";


export default {
    components: {
        TableInformation,
        TableHeaders,
        TableLine
    },
    data() {
        return {
            lines: [
                {id: 0},
                {id: 1},
                {id: 2},
                {id: 3},
            ],
            table: [
                {key: 0, unit_name: 'Мраморный щебень фр. 2-5 мм', coast: 1231, count: 1, product_name: 'Мраморный щебень', sum: 0},
                {key: 1, unit_name: 'Мраморный щебень фр. 2-5 мм', coast: 1231, count: 5, product_name: 'Мраморный щебень', sum: 0},
                {key: 2, unit_name: 'Мраморный щебень фр. 2-5 мм', coast: 1231, count: 6, product_name: 'Мраморный щебень', sum: 0},
                {key: 3, unit_name: 'Мраморный щебень фр. 2-5 мм', coast: 1231, count: 8, product_name: 'Мраморный щебень', sum: 0}
            ],
            headers: [
                {name: 'Номер', width: 60, id: 'col0'},
                {name: 'Наименование единицы', width: 230, id: 'col1'},
                {name: 'Цена', width: 80, id: 'col2'},
                {name: 'Кол-во', width: 60, id: 'col2'},
            ]
        }
    },
    methods: {
        changeWidth(id, width) {
            this.headers[id].width = width;
        },
    },
    mounted() {
        let lines = this.lines;
        let copy = [];
        for (let i = 0; i < lines.length; i++) copy.push({id: i});



        let table_sort = sortable('.table__lines', {
            placeholderClass: 'table__place_holder'
        });
        table_sort[0].addEventListener('sortupdate', function(e) {

            let serialized = sortable('.table__lines', 'serialize');
            let idx = e.detail.origin.index, _idx = e.detail.destination.index;

            console.log(idx, _idx);
            let buffer = copy[idx];
            let element = copy.splice(idx, 1);

            copy.splice(_idx, 0, toRaw(element)[0]);
            for (let i = 0; i < lines.length; i++) {
                lines[copy[i].id].id = i;
            }
        });
    }
}
</script>

<style lang="scss">
.table_block {
    margin-right: 25px;
    //width: 100%;
    max-width: 100%;

    padding: 9px 0 0 0;
    border-radius: 10px;
    box-shadow: 0 5px 20px 0 rgba(0, 0, 0, 0.07);
    border: solid 1px #eeeff1;
    background-color: #fff;

    &__settings_icon {
        margin: 0 15px 8px auto;
        width: 15px;
        height: 15px;
        background-repeat: no-repeat;
        background-image: url('../assets/settings.png');
        background-size: contain;
        background-position: right center;
    }
}
.table {
    display: flex;
    flex-direction: column;
    //justify-items: end;
    flex-wrap: nowrap;
    overflow-x: auto;
    padding-bottom: 180px;
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