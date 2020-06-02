<template>
    <div :id="wrapperId" :class="`gallery-wrapper ${use ? '' : 'not-use'}`" @wheel.shift="onWheel">
        <slot></slot>
    </div>
</template>


<script>
    import scrollBus from './ScrollBus';

    export default {
        data() {
            return{
                wrapperId: '',
                use: true,
            };
        },
        created() {
            this.wrapperId = Math.random().toString(36).substring(7);
            this._event = (msg) => {
                this.calculateBack(msg);
            };
            this._eventUse = (msg) => {
                this.use = msg;
            };
            scrollBus.$on('change', this._event);
            scrollBus.$on('use', this._eventUse);
        },
        methods: {
            calculateBack(distPerc) {
                let posOfBar = (distPerc * this.holderWidth) / 100;
                document.getElementById(this.wrapperId).scrollLeft = posOfBar;
            },
            onWheel(event) {
                document.getElementById(this.wrapperId).scrollLeft = event.deltaX;
                const percent = (event.deltaX / this.holderWidth) * 100;
                scrollBus.$emit('wheel', precent);
            },
        },
        computed: {
            holderWidth() {
                const full = document.getElementById(this.wrapperId).scrollWidth;
                const client = document.getElementById(this.wrapperId).offsetWidth;
                return full - client;
            },
        },
        beforeDestroy() {
            this._event && scrollBus && scrollBus.$off('change', this._event)
            this._eventUse && scrollBus && scrollBus.$off('use', this._eventUse)
        },
    };
</script>
