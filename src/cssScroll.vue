<template>
    <div class = "loop-wrap" :item-len="itemLen" :height="height" :loop-time="loopTime" :delay="delay">
        <div class="loop-images-container">
            <slot></slot>
        </div>
    </div>
</template>
<script>
   /*
   *   item-len 元素数量
   *   height 元素高度
   *   loop 循环周期
   *   切换时间比 0到1
   */
    export default {
        props: {
            itemLen:{
                type: Number,
                default: 10,
            },
            height:{
                type: String,
                default: '25',
            },
            loopTime:{
                type: String,
                default: '50',
            },
            delay:{
                type: Number,
                default: .9,
            }
        },
        mounted(){
            this.$el.style.height =`${this.height}px`;
            this.$el.children[0].height=`${this.itemLen * 100}%`;
            let firstEle =this.$el.children[0].firstChild.cloneNode(true);
            this.$el.children[0].appendChild(firstEle);
            this.$el.children[0].classList.add('cssAddClass');
            this.createStyle();
        },
        methods: {
            createStyle(){
                let len = this.itemLen;
                let style = document.createElement('style');
                let front = "@keyframes loop {";
                let front_webkit = "@-webkit-keyframes loop {";
                let end = "}";
                let middle = '';
                let point = 100/len;
                style.innerText = ".cssAddClass{animation: loop "+this.loopTime+"s linear infinite;-webkit-animation: loop "+this.loopTime+"s linear infinite;}";
                for(let i = 0 ; i < len*2+1; i++ ){
                    middle += (i%2 === 0 ? i/2*point:((i+this.delay)/2)*point)+"%{transform: translate3d(0,-"+(i%2 === 0 ?i/2*this.height:(i-1)/2*this.height)+"px,0);-webkit-transform: translate3d(0,-"+(i%2 === 0 ?i/2*this.height:(i-1)/2*this.height)+"px,0);}";
                }
                style.innerText += (front + middle + end)+(front_webkit + middle + end);
                document.head.appendChild(style);
            }
        }
    };
</script>
<style lang="scss" scoped>
    .loop-wrap {
        position: relative;
        width: 100%;
        overflow: hidden;
        .loop-container{
            position: absolute;
            left: 0; top: 0;
            width: 100%;
            transform: translate(0,0);
            /*animation: loop 50s linear infinite;*/
        }

    }

</style>