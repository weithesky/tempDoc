<template>
    <div ref="slider" class="slider">
        <div class="line inline"></div>
        <div ref="outline" class="line outline"></div>
        <div ref="sliderBtn" class="sliderBtn" @mousedown.stop="enterBtn($event)"></div>
        <span>{{num}}</span>
    </div>
</template>
<script>
export default {
    data () {
        return {
            disabled: false, // 是否disabled
            dragging: false, // 是否移动状态
            startWidth: 0, // 开始时outline的宽度
            startX: 0, // 点击时记录X
            currentX: 0,
            num: 0
        }
    },
    methods: {
        enterBtn (e, val) {
            if (this.disabled) return
            console.log(e)
            this.newClick(e)
            this.countSpace()
            // 监听鼠标移动和松开
            document.addEventListener('mousemove', ()=> {
                this.moveStart(event)
            }, false)
            document.addEventListener('mouseup', ()=> {
                this.moveEnd(event)
            }, false)
        },
        newClick (e) {
           this.dragging = false;
           this.startX = e.clientX
           this.startWidth = this.$refs.outline.offsetWidth
        },
        moveStart (e) {
            // if (dragging) return
            console.log('开始监听', e)
            this.dragging = true
            this.currentX = e.clientX
            console.log(this.currentX, '-', this.startX)
            let diff = this.currentX - this.startX
            this.countSpace(diff)
        },
        moveEnd (e) {
            // if (dragging) return
            this.dragging = false
            console.log('关闭监听', e)
            // 关闭监听
            document.removeEventListener('mousemove', this.moveStart, false)
            document.removeEventListener('mouseup', this.moveEnd, false)
        },
        countSpace (diff) {
           let space = 0
           let allwidth = this.$refs.slider.offsetWidth
           let newWidth = this.startWidth + diff
           if (newWidth < 0) {
               space = 0
           } else if (newWidth > newWidth) {
               space = allwidth
           } else {
               space = newWidth
           }
           this.num = space
           console.log('宽度', space)
        }
    }
}
</script>
<style scoped>
    .slider{
        width: 200px;
        height: 30px;
        position: relative;
        background: rgba(0,0,0,0.3);
    }
    .line{
        height: 4px;
        border-radius: 3px;
        position: relative;
        cursor: pointer;
    }
    .inline {
        width: 100%; 
        background-color: #e8eaec;
        top: calc(50% - 2px);
    }
    .outline{
        width: 25%;
        background: #57a3f3;
        top: calc(50% - 6px);
    }
    .sliderBtn {
        width: 12px;
        height: 12px;
        border: 2px solid  green;
        border-radius: 50%;
        background: #fff;
        position: absolute;
    }
    .sliderBtn:hover{
        transform: scale(1.2);
    }
</style>