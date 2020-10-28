<template>
    <div>
        <canvas width="1000" height="600" ref="canvasArea" class="canvas" @mousedown="start($event)"
                @dblclick="active($event)"></canvas>
        <div>current: {{mouse.x}},{{mouse.y}} <br>
            start: {{startCoords.x}},{{startCoords.y}}
            color:{{color}}

        </div>
    </div>
</template>

<script>
    export default {

        name: "Canvas",
        data: () => ({
            mouse: {
                x: 0,
                y: 0
            },
            startCoords: {
                x: 0,
                y: 0
            },
            canvas: null,
            context: null,
            windowWidth: 0,
            windowHeight: 0,
            shapes: [],
            activeShape: 0,
            corner: '',
            create: true,
            color: ''
        }),
        methods: {
            start(e) {
                let canvasPosition = this.$refs.canvasArea.getBoundingClientRect();
                this.startCoords.x = e.pageX - canvasPosition.left
                this.startCoords.y = e.pageY - canvasPosition.top
                if (this.shapes.length >= 1) {
                    for (let i = this.shapes.length - 1; i >= 0; i--) {
                        if (this.shapes[i].leftTopX >=
                            this.startCoords.x &&
                            this.shapes[i].rightTopX <=
                            this.startCoords.x &&
                            this.shapes[i].leftTopY >=
                            this.startCoords.y &&
                            this.shapes[i].bottomLeftY <=
                            this.startCoords.y) {
                            this.canvas.onmousemove = null
                            this.canvas.onmouseup = null
                        } else {
                            if (this.create == true) {
                                this.canvas.onmousemove = this.draw
                                this.canvas.onmouseup = this.end
                            } else {
                                if (((this.shapes[this.activeShape].bottomLeftX - 3 <= this.startCoords.x) &&
                                    (this.startCoords.x <= this.shapes[this.activeShape].bottomLeftX + 3)) &&
                                    ((this.shapes[this.activeShape].bottomLeftY - 3 <= this.startCoords.y) &&
                                        (this.startCoords.y <= this.shapes[this.activeShape].bottomLeftY + 3))) {
                                    this.corner = 'left-bottom'
                                }
                                if (((this.shapes[this.activeShape].leftTopX - 3 <= this.startCoords.x) &&
                                    (this.startCoords.x <= this.shapes[this.activeShape].leftTopX + 3)) &&
                                    ((this.shapes[this.activeShape].leftTopY - 3 <= this.startCoords.y) &&
                                        (this.startCoords.y <= this.shapes[this.activeShape].leftTopY + 3))) {
                                    this.corner = 'left-top'
                                }
                                if (((this.shapes[this.activeShape].bottomRightX - 3 <= this.startCoords.x) &&
                                    (this.startCoords.x <= this.shapes[this.activeShape].bottomRightX + 3)) &&
                                    ((this.shapes[this.activeShape].bottomRightY - 3 <= this.startCoords.y) &&
                                        (this.startCoords.y <= this.shapes[this.activeShape].bottomRightY + 3))) {
                                    this.corner = 'right-bottom'

                                }
                                if (((this.shapes[this.activeShape].rightTopX - 3 <= this.startCoords.x) &&
                                    (this.startCoords.x <= this.shapes[this.activeShape].rightTopX + 3)) &&
                                    ((this.shapes[this.activeShape].rightTopY - 3 <= this.startCoords.y) &&
                                        (this.startCoords.y <= this.shapes[this.activeShape].rightTopY + 3))) {
                                    this.corner = 'right-top'
                                }
                                this.canvas.onmousemove = this.change
                                this.canvas.onmouseup = this.stopChange
                            }
                        }
                    }
                } else {
                    this.canvas.onmousemove = this.draw
                    this.canvas.onmouseup = this.end
                }

            },
            draw(e) {
                let canvasPosition = this.$refs.canvasArea.getBoundingClientRect();
                this.context.clearRect(0, 0, this.canvas.width, this.canvas.height);
                this.mouse.x = e.pageX - canvasPosition.left
                this.mouse.y = e.pageY - canvasPosition.top
                if (this.shapes.length >= 1) {
                    for (let i = this.shapes.length - 1; i >= this.shapes.length - 1; i--) {
                        if (this.shapes[i].leftTopX >= this.mouse.x ||
                            this.shapes[i].bottomRightX <= this.mouse.x ||
                            this.shapes[i].leftTopY >= this.mouse.y ||
                            this.shapes[i].bottomLeftY <= this.mouse.y) {
                            this.color = 'black'
                            this.shape(
                                this.startCoords.x,
                                this.startCoords.y,
                                this.mouse.x,
                                this.startCoords.y,
                                this.mouse.x,
                                this.mouse.y,
                                this.startCoords.x,
                                this.mouse.y,
                                this.color)
                            requestAnimationFrame(this.allshapes);
                        } else {
                            this.color = 'red'
                            this.shape(
                                this.startCoords.x,
                                this.startCoords.y,
                                this.mouse.x,
                                this.startCoords.y,
                                this.mouse.x,
                                this.mouse.y,
                                this.startCoords.x,
                                this.mouse.y,
                                this.color)
                            requestAnimationFrame(this.allshapes);
                        }
                    }
                } else {
                    this.color = 'black'
                    this.shape(
                        this.startCoords.x,
                        this.startCoords.y,
                        this.mouse.x,
                        this.startCoords.y,
                        this.mouse.x,
                        this.mouse.y,
                        this.startCoords.x,
                        this.mouse.y,
                        this.color)
                    requestAnimationFrame(this.allshapes);
                }
            },
            end() {
                if (this.shapes.length >= 1) {
                    if (this.shapes[this.shapes.length - 1].bottomRightX !== this.mouse.x &&
                        this.shapes[this.shapes.length - 1].bottomRightY !== this.mouse.y &&
                        this.startCoords.x !== this.mouse.x &&
                        this.startCoords.y !== this.mouse.y) {
                        this.shapes.push({
                            leftTopX: this.startCoords.x,
                            leftTopY: this.startCoords.y,
                            rightTopX: this.mouse.x,
                            rightTopY: this.startCoords.y,
                            bottomRightX: this.mouse.x,
                            bottomRightY: this.mouse.y,
                            bottomLeftX: this.startCoords.x,
                            bottomLeftY: this.mouse.y,
                            color: this.color
                        })
                    }
                } else {
                    if (this.startCoords.x !== this.mouse.x &&
                        this.startCoords.y !== this.mouse.y) {
                        this.create = true
                        this.shapes.push({
                            leftTopX: this.startCoords.x,
                            leftTopY: this.startCoords.y,
                            rightTopX: this.mouse.x,
                            rightTopY: this.startCoords.y,
                            bottomRightX: this.mouse.x,
                            bottomRightY: this.mouse.y,
                            bottomLeftX: this.startCoords.x,
                            bottomLeftY: this.mouse.y,
                            color: this.color
                        })
                    }
                }
                this.canvas.onmousedown = null
                this.canvas.onmousemove = null
            },
            allshapes() {
                for (let i = 0; i < this.shapes.length; i++) {
                    this.shape(
                        this.shapes[i].leftTopX,
                        this.shapes[i].leftTopY,
                        this.shapes[i].rightTopX,
                        this.shapes[i].rightTopY,
                        this.shapes[i].bottomRightX,
                        this.shapes[i].bottomRightY,
                        this.shapes[i].bottomLeftX,
                        this.shapes[i].bottomLeftY,
                        this.shapes[i].color)
                }
            },
            active(e) {
                this.create = !this.create
                let canvasPosition = this.$refs.canvasArea.getBoundingClientRect();
                let x = e.pageX - canvasPosition.left
                let y = e.pageY - canvasPosition.top
                for (let i = 0; i <= this.shapes.length - 1; i++) {
                    if (this.shapes[i].color != 'red') {
                        this.context.clearRect(0, 0, this.canvas.width, this.canvas.height);
                        this.shapes[i].color = 'black'
                        if (this.shapes[i].leftTopX < x &&
                            this.shapes[i].rightTopX > x &&
                            this.shapes[i].leftTopY < y &&
                            this.shapes[i].bottomRightY > y) {
                            this.activeShape = i
                            this.shapes[this.activeShape].color = 'green'
                            requestAnimationFrame(this.allshapes);
                        } else {
                            this.context.clearRect(0, 0, this.canvas.width, this.canvas.height);
                            this.shapes[i].color = 'black'
                        }
                    }
                    requestAnimationFrame(this.allshapes);
                }
            },
            shape(x1, y1, x2, y2, x3, y3, x4, y4, color) {
                if (color == 'green') {
                    this.context.beginPath()
                    this.context.arc(x1, y1, 3, 0, 2 * Math.PI)
                    this.context.strokeStyle = color
                    this.context.stroke();
                    this.context.beginPath()
                    this.context.arc(x2, y2, 3, 0, 2 * Math.PI)
                    this.context.strokeStyle = color
                    this.context.stroke();
                    this.context.beginPath()
                    this.context.arc(x3, y3, 3, 0, 2 * Math.PI)
                    this.context.strokeStyle = color
                    this.context.stroke();
                    this.context.beginPath()
                    this.context.arc(x4, y4, 3, 0, 2 * Math.PI)
                    this.context.strokeStyle = color
                    this.context.stroke();
                }
                this.context.beginPath()
                this.context.moveTo(x1, y1);
                this.context.lineTo(x2, y2)
                this.context.lineTo(x3, y3)
                this.context.lineTo(x4, y4)
                this.context.lineTo(x1, y1)
                this.context.closePath()
                this.context.lineWidth = 3;
                this.context.strokeStyle = color
                this.context.stroke();
            },
            change(e) {
                let canvasPosition = this.$refs.canvasArea.getBoundingClientRect();
                this.context.clearRect(0, 0, this.canvas.width, this.canvas.height);
                this.mouse.x = e.pageX - canvasPosition.left
                this.mouse.y = e.pageY - canvasPosition.top
                this.context.clearRect(0, 0, this.canvas.width, this.canvas.height);
                if (this.corner == 'right-top') {
                    this.shapes[this.activeShape].rightTopX = this.mouse.x
                    this.shapes[this.activeShape].rightTopY = this.mouse.y
                }
                if (this.corner == 'left-top') {
                    this.shapes[this.activeShape].leftTopX = this.mouse.x
                    this.shapes[this.activeShape].leftTopY = this.mouse.y
                }
                if (this.corner == 'right-bottom') {
                    this.shapes[this.activeShape].bottomRightX = this.mouse.x
                    this.shapes[this.activeShape].bottomRightY = this.mouse.y
                }
                if (this.corner == 'left-bottom') {
                    this.shapes[this.activeShape].bottomLeftX = this.mouse.x
                    this.shapes[this.activeShape].bottomLeftY = this.mouse.y
                }
                if (this.shapes.length >= 1) {
                    for (let i =  this.shapes.length - 1; i >= 0; i--) {
                        if (this.shapes[i].leftTopX < this.mouse.x &&
                            this.shapes[i].rightTopX > this.mouse.x &&
                            this.shapes[i].leftTopY < this.mouse.y &&
                            this.shapes[i].bottomRightY > this.mouse.y) {
                            this.context.clearRect(0, 0, this.canvas.width, this.canvas.height);
                            this.color = 'red'
                            this.shape(
                                this.shapes[this.activeShape].leftTopX,
                                this.shapes[this.activeShape].leftTopY,
                                this.shapes[this.activeShape].rightTopX,
                                this.shapes[this.activeShape].rightTopY,
                                this.shapes[this.activeShape].bottomRightX,
                                this.shapes[this.activeShape].bottomRightY,
                                this.shapes[this.activeShape].bottomLeftX,
                                this.shapes[this.activeShape].bottomLeftY,
                                this.color)
                            this.allshapes()
                        } else {
                            this.color = 'green'
                            this.context.clearRect(0, 0, this.canvas.width, this.canvas.height);
                            this.shape(
                                this.shapes[this.activeShape].leftTopX,
                                this.shapes[this.activeShape].leftTopY,
                                this.shapes[this.activeShape].rightTopX,
                                this.shapes[this.activeShape].rightTopY,
                                this.shapes[this.activeShape].bottomRightX,
                                this.shapes[this.activeShape].bottomRightY,
                                this.shapes[this.activeShape].bottomLeftX,
                                this.shapes[this.activeShape].bottomLeftY,
                                this.color)
                            this.allshapes()
                        }
                    }
                } else {
                    this.color = 'green'
                    this.context.clearRect(0, 0, this.canvas.width, this.canvas.height);
                    this.shape(
                        this.shapes[this.activeShape].leftTopX,
                        this.shapes[this.activeShape].leftTopY,
                        this.shapes[this.activeShape].rightTopX,
                        this.shapes[this.activeShape].rightTopY,
                        this.shapes[this.activeShape].bottomRightX,
                        this.shapes[this.activeShape].bottomRightY,
                        this.shapes[this.activeShape].bottomLeftX,
                        this.shapes[this.activeShape].bottomLeftY,
                        this.color)
                    this.allshapes()
                }
            },
            stopChange() {
                this.allshapes()
                this.context.clearRect(0, 0, this.canvas.width, this.canvas.height);
                if (this.corner == 'right-top') {
                    this.shapes[this.activeShape].rightTopX = this.mouse.x
                    this.shapes[this.activeShape].rightTopY = this.mouse.y
                }
                if (this.corner == 'left-top') {
                    this.shapes[this.activeShape].leftTopX = this.mouse.x
                    this.shapes[this.activeShape].leftTopY = this.mouse.y
                }
                if (this.corner == 'right-bottom') {
                    this.shapes[this.activeShape].bottomRightX = this.mouse.x
                    this.shapes[this.activeShape].bottomRightY = this.mouse.y
                }
                if (this.corner == 'left-bottom') {
                    this.shapes[this.activeShape].bottomLeftX = this.mouse.x
                    this.shapes[this.activeShape].bottomLeftY = this.mouse.y
                }
                this.shapes[this.activeShape].color = this.color
                this.allshapes()
                this.canvas.onmousedown = null
                this.canvas.onmousemove = null
            },

        },
        mounted() {
            this.canvas = this.$refs.canvasArea
            this.context = this.canvas.getContext("2d")
            this.windowWidth = document.body.clientWidth;
            this.windowHeight = document.body.clientHeight;

        },
        updated() {
            this.allshapes()
        }
    }
</script>

<style scoped>
    .canvas {
        border: 1px solid black;

    }


</style>
