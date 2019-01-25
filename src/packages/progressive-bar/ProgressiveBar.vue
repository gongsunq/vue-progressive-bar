<template>
    <div class="progressive-bar-box" v-if="backs">
        <div class="pb-bg" :style="bgStyle">
            <div slot="content"></div>
        </div>
    </div>
</template>

<script>

    export default {
        name: "ProgressiveBar",
        data() {
            return {
                bgStyle: "background-color: transparent",
                bgColor: [255, 255, 255]
            };
        },
        components: {
            
        },
        props: {
            start: {
                type: Number,
                required: true
            },
            end: {
                type: Number,
                required: true
            },
            color: Array
        },
        methods: {

            /**
             * 监听滚动条
             */ 
            monitor() {
                let current = document.body.scrollTop || document.documentElement.scrollTop;
                this.setPbBg(current, this.start, this.end, this.color);
            },

            /**
             * progressive-bar根据位置渐变透明度
             * param current {number}        [当前滚动条位置]
             * param start {number}          [开始位置]
             * param end   {number}          [结束位置]
             * param color {string}          [结束位置]
             */
            setPbBg(current, start, end, color) {
                if(typeof start === "number" && start >= 0 && typeof end === "number" && end >= 0) {

                    // 设置progressive-bar颜色
                    let colorString = color && typeof color === "object" ? color.join(", ") : this.bgColor.join(", ");

                    // 判断参数，设置背景透明度
                    if(current >= start && current <= end) {
                        let a = (current - start) / (end - start);
                        this.bgStyle = "background-color: rgba(" + colorString + ", " + a + ")";
                    } else if(current < start) {
                        this.bgStyle = "background-color: transparent";
                    } else if(current > end) {
                        this.bgStyle = "background-color: rgba(" + colorString + ", 1)";
                    }
                }
            }
        },
        created() {

            // 监听滚动条，到达位置处理progressive-bar
            window.onscroll = this.monitor;
        }
    };
</script>