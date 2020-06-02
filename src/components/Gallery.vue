<template>
    <div :id="wrapperId" class="gallery-wrapper">
        <slot></slot>
    </div>
</template>


<script>
    import scrollBus from './ScrollBus';

    export default {
        data() {
            return{
                wrapperId: '',
            };
        },
        created() {
            this.wrapperId = Math.random().toString(36).substring(7);
            this._event = (msg) => {
                console.log(msg);
                this.calculateBack(msg);
            };
            scrollBus.$on('change', this._event);
        },
        methods: {
            calculateBack(distPerc) {
                let posOfBar = (distPerc * this.holderWidth) / 100;
                console.log(posOfBar);
                document.getElementById(this.wrapperId).scrollLeft = posOfBar;
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
        },
    };
</script>
