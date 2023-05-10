<template>
    <div class="headers">
        <div v-show="!mobile" class="headers__content" v-for="(header, idx) in headers">
            <div v-show="header.active" class="headers__column" :style="{width : header.width+'px'}">
                <p class="headers__text"><strong>{{header.name}}</strong></p>
            </div>
        </div>
    </div>
</template>

<script>

export default {
    props: {
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
        let elements = document.getElementsByClassName('headers__column');
        this.createEvent(elements);
    },
    data() {
        return {columns: []}
    },
    methods: {
        createEvent(elements) {
            const emit = this.$emit;
            let props = this.$props;
            for (let i = 0; i < this.$props.headers.length; i++) {
                const element = elements[i];
                const id = i;
                let startX = 0;
                let startWidth = props.headers[id].width;
                element.addEventListener('mousedown', function (event) {
                    startX = event.clientX;
                    startWidth = props.headers[id].width
                    document.documentElement.addEventListener('mouseup', stopResize, false);
                    document.documentElement.addEventListener('mousemove', resize, false);
                });

                let resize = function (event) {
                    emit('updateWidth', id, startWidth+event.clientX-startX);
                }
                let stopResize = function (event) {
                    document.documentElement.removeEventListener('mousemove', resize);
                }
            }
        },
    }
}
</script>

<style lang="scss" scoped>
.headers {
    @media (min-width: 768px) {
        height: 42px;
    }
    display: flex;
    border-bottom: solid 1px #eeeff1;
    border-top: solid 1px #eeeff1;
    width: max-content;
    &__column {
        border-right: solid 1px #eeeff1;
        padding-left: 10px;
        height: 42px;
        display: flex;
        align-items: center;
        overflow:hidden;
        white-space:nowrap;
        text-overflow: ellipsis;
        justify-content: space-between;
    }
    //&__content:last-child {
    //    .headers__column {
    //        border-right: none;
    //    }
    //}
    &__text {
        font-size: 16px;
        font-weight: 600;
        font-stretch: normal;
        font-style: normal;
        line-height: normal;
        letter-spacing: normal;
        color: #000;
    }
    &__drag {
        min-width: 4px;
        height: 100%;
        &:hover {
            background-color: #eeeff1;
        }
    }
}
</style>