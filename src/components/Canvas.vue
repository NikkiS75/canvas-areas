<template>
    <div>
    <canvas  width="1000" height="600" ref="canvasArea" class="canvas" @mousedown="start($event)" ></canvas>
        <div>current: {{mouse.x}},{{mouse.y}} <br>
            start: {{startCoords.x}},{{startCoords.y}}

        </div>
    </div>
</template>

<script>
    export default {

        name: "Canvas",
        data: () => ({
            mouse:{
                x:0,
                y:0
            },
            startCoords:{
                x:0,
                y:0
            },
            canvas: null,
            context: null,
            windowWidth:0,
            windowHeight:0,
            shape:{
                startX:0,
                startY:0,
                endX:0,
                endY:0
            },
            shapes:[]
        }),
        methods:{
            start(e){
                let canvasPosition = this.$refs.canvasArea.getBoundingClientRect();
                this.startCoords.x = e.pageX - canvasPosition.left
                this.startCoords.y = e.pageY - canvasPosition.top
                this.canvas.onmousemove = this.draw
                this.canvas.onmouseup = this.end
            },
            draw(e){
                let canvasPosition = this.$refs.canvasArea.getBoundingClientRect();
                this.context.clearRect(0,0,this.canvas.width,this.canvas.height);
                this.context.beginPath()
                this.context.moveTo(this.startCoords.x, this.startCoords.y);
              //  this.context.arc(this.startCoords.x, this.startCoords.y, 2, 0, 2 * Math.PI)
                this.mouse.x = e.pageX - canvasPosition.left
                this.mouse.y = e.pageY - canvasPosition.top
                this.context.lineTo(this.mouse.x , this.startCoords.y)
              // this.context.arc(this.mouse.x -2  , this.startCoords.y, 2, 0, 3 * Math.PI)
                this.context.lineTo(this.mouse.x, this.mouse.y)
             //   this.context.arc(this.mouse.x, this.mouse.y, 2, 0, 2 * Math.PI)
                this.context.lineTo(this.startCoords.x -2 , this.mouse.y)
              // this.context.arc(this.startCoords.x  , this.mouse.y, 2, 0, 2 * Math.PI)
                this.context.closePath()
                this.context.strokeStyle = 'black';
                this.context.lineCap = 'round';
                this.context.lineJoin = "round";
                this.context.lineWidth = 4;
                this.context.stroke();
                requestAnimationFrame(this.allshapes);
            },
            end(){
                this.shape.startX = this.startCoords.x
                this.shape.endX = this.mouse.x
                this.shape.startY = this.startCoords.y
                this.shape.endY = this.mouse.y
                this.shapes.push({startX: this.startCoords.x, startY:this.startCoords.y, endX:this.mouse.x, endY: this.mouse.y})
                console.log(this.shapes)
                this.canvas.onmousedown = null
                this.canvas.onmousemove = null
            },
            allshapes(){
                for(let i=0 ; i<this.shapes.length;i++){
                    this.context.moveTo(this.shapes[i].startX, this.shapes[i].startY);
                  // this.context.arc(this.shapes[i].startX, this.shapes[i].startY, 2, 0, 2 * Math.PI)
                    this.context.lineTo(this.shapes[i].endX , this.shapes[i].startY)
                  // this.context.arc(this.shapes[i].endX - 2 ,  this.shapes[i].startY, 2, 0, 2 * Math.PI)
                    this.context.lineTo(this.shapes[i].endX,this.shapes[i].endY)
                 // this.context.arc(this.shapes[i].endX, this.shapes[i].endY, 2, 0, 2 * Math.PI)
                    this.context.lineTo(this.shapes[i].startX , this.shapes[i].endY)
                   // this.context.arc(this.shapes[i].startX -2 , this.shapes[i].endY, 2, 0, 2 * Math.PI)
                    this.context.closePath()
                    this.context.strokeStyle = 'black';
                    this.context.lineJoin = "round";
                    this.context.lineCap = 'round';
                    this.context.lineWidth = 3;
                    this.context.stroke();
                }
            }

        },
        mounted()
        {
            this.canvas = this.$refs.canvasArea
            this.context = this.canvas.getContext("2d")
            this.windowWidth = document.body.clientWidth;
            this.windowHeight = document.body.clientHeight;

        },
        updated() {

        }
    }
</script>

<style scoped>
    .canvas{
        border: 1px solid black;

    }


</style>
