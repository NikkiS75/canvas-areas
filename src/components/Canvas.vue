<template>
    <div>
        <canvas :width="windowWidth" :height="windowHeight" ref="canvasArea" class="canvas" @mousedown="start($event)"></canvas>
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
            state: '',
            over:false,
        }),
        methods: {
            start(e) {
                let canvasPosition = this.$refs.canvasArea.getBoundingClientRect();
                this.startCoords.x = e.pageX - canvasPosition.left
                this.startCoords.y = e.pageY - canvasPosition.top
                this.mouse.x = e.pageX - canvasPosition.left
                this.mouse.y = e.pageY - canvasPosition.top
                if (this.shapes.length >= 1) {
                    for (let i = 0; i <= this.shapes.length-1; i++) {
                        if (this.startCoords.x >= this.shapes[i].leftTopX   &&
                            this.startCoords.x <= this.shapes[i].rightTopX  &&
                            this.startCoords.y >= this.shapes[i].leftTopY  &&
                            this.startCoords.y <= this.shapes[i].bottomLeftY )
                        {
                            requestAnimationFrame(this.allshapes);
                            if (((this.shapes[this.activeShape].bottomLeftX - 10 <= this.startCoords.x) &&
                                (this.startCoords.x <= this.shapes[this.activeShape].bottomLeftX + 10)) &&
                                ((this.shapes[this.activeShape].bottomLeftY - 10 <= this.startCoords.y) &&
                                    (this.startCoords.y <= this.shapes[this.activeShape].bottomLeftY + 10))) {
                                this.corner = 'left-bottom'
                                this.shapes[this.activeShape].active = true
                                this.canvas.onmousemove = this.change
                                this.canvas.onmouseup = this.stopChange
                            }else
                            if (((this.shapes[this.activeShape].leftTopX - 10 <= this.startCoords.x) &&
                                (this.startCoords.x <= this.shapes[this.activeShape].leftTopX + 10)) &&
                                ((this.shapes[this.activeShape].leftTopY - 10 <= this.startCoords.y) &&
                                    (this.startCoords.y <= this.shapes[this.activeShape].leftTopY + 10))) {
                                this.corner = 'left-top'
                                this.shapes[this.activeShape].active = true
                                this.canvas.onmousemove = this.change
                                this.canvas.onmouseup = this.stopChange
                            } else
                            if (((this.shapes[this.activeShape].bottomRightX - 10 <= this.startCoords.x) &&
                                (this.startCoords.x <= this.shapes[this.activeShape].bottomRightX + 10)) &&
                                ((this.shapes[this.activeShape].bottomRightY - 10 <= this.startCoords.y) &&
                                    (this.startCoords.y <= this.shapes[this.activeShape].bottomRightY + 10))) {
                                this.corner = 'right-bottom'
                                this.shapes[this.activeShape].active = true
                                this.canvas.onmousemove = this.change
                                this.canvas.onmouseup = this.stopChange

                            } else
                            if (((this.shapes[this.activeShape].rightTopX - 10 <= this.startCoords.x) &&
                                (this.startCoords.x <= this.shapes[this.activeShape].rightTopX + 10)) &&
                                ((this.shapes[this.activeShape].rightTopY - 10 <= this.startCoords.y) &&
                                    (this.startCoords.y <= this.shapes[this.activeShape].rightTopY + 10))) {
                                this.corner = 'right-top'
                                this.shapes[this.activeShape].active = true
                                this.canvas.onmousemove = this.change
                                this.canvas.onmouseup = this.stopChange
                            } else{
                                this.activeShape = i
                                this.shapes[this.activeShape].active = true
                                for (let i = this.shapes.length - 1; i >= 0; i--) {
                                    if (i != this.activeShape){
                                        this.shapes[i].active = false
                                        this.context.clearRect(0, 0, this.canvas.width, this.canvas.height);
                                        this.allshapes()
                                    }}
                            this.canvas.onmousemove = null
                            this.canvas.onmouseup = null}
                            return
                        } else {
                            if (((this.shapes[this.activeShape].bottomLeftX - 10 <= this.startCoords.x) &&
                                (this.startCoords.x <= this.shapes[this.activeShape].bottomLeftX + 10)) &&
                                ((this.shapes[this.activeShape].bottomLeftY - 10 <= this.startCoords.y) &&
                                    (this.startCoords.y <= this.shapes[this.activeShape].bottomLeftY + 10))) {
                                this.corner = 'left-bottom'
                                this.shapes[this.activeShape].state = 'active'
                                this.canvas.onmousemove = this.change
                                this.canvas.onmouseup = this.stopChange
                            } else if (((this.shapes[this.activeShape].leftTopX - 10 <= this.startCoords.x) &&
                                (this.startCoords.x <= this.shapes[this.activeShape].leftTopX + 10)) &&
                                ((this.shapes[this.activeShape].leftTopY - 3 <= this.startCoords.y) &&
                                    (this.startCoords.y <= this.shapes[this.activeShape].leftTopY + 10))) {
                                this.corner = 'left-top'
                                this.canvas.onmousemove = this.change
                                this.canvas.onmouseup = this.stopChange
                            } else if (((this.shapes[this.activeShape].bottomRightX - 10 <= this.startCoords.x) &&
                                (this.startCoords.x <= this.shapes[this.activeShape].bottomRightX + 10)) &&
                                ((this.shapes[this.activeShape].bottomRightY - 10 <= this.startCoords.y) &&
                                    (this.startCoords.y <= this.shapes[this.activeShape].bottomRightY + 10))) {
                                this.corner = 'right-bottom'
                                this.canvas.onmousemove = this.change
                                this.canvas.onmouseup = this.stopChange

                            } else if (((this.shapes[this.activeShape].rightTopX - 10 <= this.startCoords.x) &&
                                (this.startCoords.x <= this.shapes[this.activeShape].rightTopX + 10)) &&
                                ((this.shapes[this.activeShape].rightTopY - 10 <= this.startCoords.y) &&
                                    (this.startCoords.y <= this.shapes[this.activeShape].rightTopY + 10))) {
                                this.corner = 'right-top'
                                this.canvas.onmousemove = this.change
                                this.canvas.onmouseup = this.stopChange
                            } else {
                                this.canvas.onmousemove = this.draw
                                this.canvas.onmouseup = this.end
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
                    for (let i = this.shapes.length-1; i >=0; i--) {
                        if( this.intersect(
                            this.startCoords.x,
                            this.startCoords.y,
                            this.startCoords.x,
                            this.mouse.y,
                            this.shapes[i].leftTopX,
                            this.shapes[i].leftTopY,
                            this.shapes[i].bottomLeftX,
                            this.shapes[i].bottomLeftY)
                            ||this.intersect(
                                this.startCoords.x,
                                this.startCoords.y,
                                this.startCoords.x,
                                this.mouse.y,
                                this.shapes[i].leftTopX,
                                this.shapes[i].leftTopY,
                                this.shapes[i].rightTopX,
                                this.shapes[i].rightTopY )
                            ||this.intersect(
                                this.startCoords.x,
                                this.startCoords.y,
                                this.startCoords.x,
                                this.mouse.y,
                                this.shapes[i].rightTopX,
                                this.shapes[i].rightTopY,
                                this.shapes[i].bottomRightX,
                                this.shapes[i].bottomRightY)
                            ||this.intersect(
                                this.startCoords.x,
                                this.startCoords.y,
                                this.startCoords.x,
                                this.mouse.y,
                                this.shapes[i].bottomRightX,
                                this.shapes[i].bottomRightY,
                                this.shapes[i].bottomLeftX,
                                this.shapes[i].bottomLeftY) // left-line
                            ||
                            this.intersect(
                                this.startCoords.x,
                                this.startCoords.y,
                                this.mouse.x,
                                this.startCoords.y,
                                this.shapes[i].leftTopX,
                                this.shapes[i].leftTopY,
                                this.shapes[i].bottomLeftX,
                                this.shapes[i].bottomLeftY)
                            || this.intersect(
                                this.startCoords.x,
                                this.startCoords.y,
                                this.mouse.x,
                                this.startCoords.y,
                                this.shapes[i].leftTopX,
                                this.shapes[i].leftTopY,
                                this.shapes[i].rightTopX,
                                this.shapes[i].rightTopY)
                            ||this.intersect(
                                this.startCoords.x,
                                this.startCoords.y,
                                this.mouse.x,
                                this.startCoords.y,
                                this.shapes[i].rightTopX,
                                this.shapes[i].rightTopY,
                                this.shapes[i].bottomRightX,
                                this.shapes[i].bottomRightY)
                            ||  this.intersect(
                                this.startCoords.x,
                                this.startCoords.y,
                                this.mouse.x,
                                this.startCoords.y,
                                this.shapes[i].bottomRightX,
                                this.shapes[i].bottomRightY,
                                this.shapes[i].bottomLeftX,
                                this.shapes[i].bottomLeftY  //top-line
                            )
                            || this.intersect(
                                this.mouse.x,
                                this.startCoords.y,
                                this.mouse.x,
                                this.mouse.y,
                                this.shapes[i].leftTopX,
                                this.shapes[i].leftTopY,
                                this.shapes[i].bottomLeftX,
                                this.shapes[i].bottomLeftY)
                            || this.intersect(
                                this.mouse.x,
                                this.startCoords.y,
                                this.mouse.x,
                                this.mouse.y,
                                this.shapes[i].leftTopX,
                                this.shapes[i].leftTopY,
                                this.shapes[i].rightTopX,
                                this.shapes[i].rightTopY)
                            ||this.intersect(
                                this.mouse.x,
                                this.startCoords.y,
                                this.mouse.x,
                                this.mouse.y,
                                this.shapes[i].rightTopX,
                                this.shapes[i].rightTopY,
                                this.shapes[i].bottomRightX,
                                this.shapes[i].bottomRightY)
                            ||  this.intersect(
                                this.mouse.x,
                                this.startCoords.y,
                                this.mouse.x,
                                this.mouse.y,
                                this.shapes[i].bottomRightX,
                                this.shapes[i].bottomRightY,
                                this.shapes[i].bottomLeftX,
                                this.shapes[i].bottomLeftY  //right-line
                            )
                            || this.intersect(
                                this.startCoords.x,
                                this.mouse.y,
                                this.mouse.x,
                                this.mouse.y,
                                this.shapes[i].leftTopX,
                                this.shapes[i].leftTopY,
                                this.shapes[i].bottomLeftX,
                                this.shapes[i].bottomLeftY)
                            || this.intersect(
                                this.startCoords.x,
                                this.mouse.y,
                                this.mouse.x,
                                this.mouse.y,
                                this.shapes[i].leftTopX,
                                this.shapes[i].leftTopY,
                                this.shapes[i].rightTopX,
                                this.shapes[i].rightTopY)
                            ||this.intersect(
                                this.startCoords.x,
                                this.mouse.y,
                                this.mouse.x,
                                this.mouse.y,
                                this.shapes[i].rightTopX,
                                this.shapes[i].rightTopY,
                                this.shapes[i].bottomRightX,
                                this.shapes[i].bottomRightY)
                            ||  this.intersect(
                                this.startCoords.x,
                                this.mouse.y,
                                this.mouse.x,
                                this.mouse.y,
                                this.shapes[i].bottomRightX,
                                this.shapes[i].bottomRightY,
                                this.shapes[i].bottomLeftX,
                                this.shapes[i].bottomLeftY  //bottom-line
                            )) {
                            this.over = true
                            this.shape(
                                this.startCoords.x,
                                this.startCoords.y,
                                this.mouse.x,
                                this.startCoords.y,
                                this.mouse.x,
                                this.mouse.y,
                                this.startCoords.x,
                                this.mouse.y,
                                false,
                                this.over,
                            )
                            requestAnimationFrame(this.allshapes);
                            break;
                        } else {
                            this.over = false
                            this.shape(
                                this.startCoords.x,
                                this.startCoords.y,
                                this.mouse.x,
                                this.startCoords.y,
                                this.mouse.x,
                                this.mouse.y,
                                this.startCoords.x,
                                this.mouse.y,
                                false,
                                this.over
                            )
                            requestAnimationFrame(this.allshapes);
                        }
                    }
                } else {
                    this.over = false
                    this.shape(
                        this.startCoords.x,
                        this.startCoords.y,
                        this.mouse.x,
                        this.startCoords.y,
                        this.mouse.x,
                        this.mouse.y,
                        this.startCoords.x,
                        this.mouse.y,
                        false,
                        this.over)
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
                            active: false,
                            over: this.over
                        })
                    }
                } else {
                    if ((this.startCoords.x !== this.mouse.x &&
                        this.startCoords.y !== this.mouse.y)) {
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
                            active: false,
                            over: false
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
                        this.shapes[i].active,
                        this.shapes[i].over
                    )
                }
            },
            shape(x1, y1, x2, y2, x3, y3, x4, y4, active, over) {
                if (active == true) {
                    this.context.beginPath()
                    this.context.arc(x1, y1, 3, 0, 2 * Math.PI)
                    this.context.strokeStyle = 'green'
                    this.context.stroke();
                    this.context.beginPath()
                    this.context.arc(x2, y2, 3, 0, 2 * Math.PI)
                    this.context.strokeStyle = 'green'
                    this.context.stroke();
                    this.context.beginPath()
                    this.context.arc(x3, y3, 3, 0, 2 * Math.PI)
                    this.context.strokeStyle = 'green'
                    this.context.stroke();
                    this.context.beginPath()
                    this.context.arc(x4, y4, 3, 0, 2 * Math.PI)
                    this.context.strokeStyle = 'green'
                    this.context.stroke();
                    this.context.beginPath()
                    this.context.moveTo(x1, y1);
                    this.context.lineTo(x2, y2)
                    this.context.lineTo(x3, y3)
                    this.context.lineTo(x4, y4)
                    this.context.lineTo(x1, y1)
                    this.context.closePath()
                    this.context.lineWidth = 3;
                    this.context.strokeStyle = 'green'
                    this.context.stroke();
                }
                if (over == true) {
                    this.context.beginPath()
                    this.context.arc(x1, y1, 3, 0, 2 * Math.PI)
                    this.context.strokeStyle = 'red'
                    this.context.stroke();
                    this.context.beginPath()
                    this.context.arc(x2, y2, 3, 0, 2 * Math.PI)
                    this.context.strokeStyle = 'red'
                    this.context.stroke();
                    this.context.beginPath()
                    this.context.arc(x3, y3, 3, 0, 2 * Math.PI)
                    this.context.strokeStyle = 'red'
                    this.context.stroke();
                    this.context.beginPath()
                    this.context.arc(x4, y4, 3, 0, 2 * Math.PI)
                    this.context.strokeStyle = 'red'
                    this.context.stroke();
                    this.context.beginPath()
                    this.context.moveTo(x1, y1);
                    this.context.lineTo(x2, y2)
                    this.context.lineTo(x3, y3)
                    this.context.lineTo(x4, y4)
                    this.context.lineTo(x1, y1)
                    this.context.closePath()
                    this.context.lineWidth = 3;
                    this.context.strokeStyle = 'red'
                    this.context.stroke();
                }
                if (active == false && over == false){
                    this.context.beginPath()
                    this.context.moveTo(x1, y1);
                    this.context.lineTo(x2, y2)
                    this.context.lineTo(x3, y3)
                    this.context.lineTo(x4, y4)
                    this.context.lineTo(x1, y1)
                    this.context.closePath()
                    this.context.lineWidth = 3;
                    this.context.strokeStyle = 'black'
                    this.context.stroke();
                }
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
                           // this.context.clearRect(0, 0, this.canvas.width, this.canvas.height);
                            if (i != this.activeShape){
                            if( this.intersect(
                                this.shapes[this.activeShape].leftTopX,
                                this.shapes[this.activeShape].leftTopY,
                                this.shapes[this.activeShape].bottomLeftX,
                                this.shapes[this.activeShape].bottomLeftY,
                                this.shapes[i].leftTopX,
                                this.shapes[i].leftTopY,
                                this.shapes[i].bottomLeftX,
                                this.shapes[i].bottomLeftY)
                             ||this.intersect(
                                this.shapes[this.activeShape].leftTopX,
                                this.shapes[this.activeShape].leftTopY,
                                this.shapes[this.activeShape].bottomLeftX,
                                this.shapes[this.activeShape].bottomLeftY,
                                this.shapes[i].leftTopX,
                                this.shapes[i].leftTopY,
                                this.shapes[i].rightTopX,
                                this.shapes[i].rightTopY )
                              ||this.intersect(
                                this.shapes[this.activeShape].leftTopX,
                                this.shapes[this.activeShape].leftTopY,
                                this.shapes[this.activeShape].bottomLeftX,
                                this.shapes[this.activeShape].bottomLeftY,
                                this.shapes[i].rightTopX,
                                this.shapes[i].rightTopY,
                                this.shapes[i].bottomRightX,
                                this.shapes[i].bottomRightY)
                               ||this.intersect(
                                    this.shapes[this.activeShape].leftTopX,
                                    this.shapes[this.activeShape].leftTopY,
                                    this.shapes[this.activeShape].bottomLeftX,
                                    this.shapes[this.activeShape].bottomLeftY,
                                    this.shapes[i].bottomRightX,
                                    this.shapes[i].bottomRightY,
                                    this.shapes[i].bottomLeftX,
                                    this.shapes[i].bottomLeftY) // left-line
                                ||
                                this.intersect(
                                        this.shapes[this.activeShape].leftTopX,
                                        this.shapes[this.activeShape].leftTopY,
                                        this.shapes[this.activeShape].rightTopX,
                                        this.shapes[this.activeShape].rightTopY,
                                        this.shapes[i].leftTopX,
                                        this.shapes[i].leftTopY,
                                        this.shapes[i].bottomLeftX,
                                        this.shapes[i].bottomLeftY)
                                || this.intersect(
                                        this.shapes[this.activeShape].leftTopX,
                                        this.shapes[this.activeShape].leftTopY,
                                        this.shapes[this.activeShape].rightTopX,
                                        this.shapes[this.activeShape].rightTopY,
                                        this.shapes[i].leftTopX,
                                        this.shapes[i].leftTopY,
                                        this.shapes[i].rightTopX,
                                        this.shapes[i].rightTopY)
                                ||this.intersect(
                                        this.shapes[this.activeShape].leftTopX,
                                        this.shapes[this.activeShape].leftTopY,
                                        this.shapes[this.activeShape].rightTopX,
                                        this.shapes[this.activeShape].rightTopY,
                                        this.shapes[i].rightTopX,
                                        this.shapes[i].rightTopY,
                                        this.shapes[i].bottomRightX,
                                        this.shapes[i].bottomRightY)
                                ||  this.intersect(
                                        this.shapes[this.activeShape].leftTopX,
                                        this.shapes[this.activeShape].leftTopY,
                                        this.shapes[this.activeShape].rightTopX,
                                        this.shapes[this.activeShape].rightTopY,
                                        this.shapes[i].bottomRightX,
                                        this.shapes[i].bottomRightY,
                                        this.shapes[i].bottomLeftX,
                                        this.shapes[i].bottomLeftY  //top-line
                                    )
                                || this.intersect(
                                    this.shapes[this.activeShape].rightTopX,
                                    this.shapes[this.activeShape].rightTopY,
                                    this.shapes[this.activeShape].bottomRightX,
                                    this.shapes[this.activeShape].bottomRightY,
                                    this.shapes[i].leftTopX,
                                    this.shapes[i].leftTopY,
                                    this.shapes[i].bottomLeftX,
                                    this.shapes[i].bottomLeftY)
                                || this.intersect(
                                    this.shapes[this.activeShape].rightTopX,
                                    this.shapes[this.activeShape].rightTopY,
                                    this.shapes[this.activeShape].bottomRightX,
                                    this.shapes[this.activeShape].bottomRightY,
                                    this.shapes[i].leftTopX,
                                    this.shapes[i].leftTopY,
                                    this.shapes[i].rightTopX,
                                    this.shapes[i].rightTopY)
                                ||this.intersect(
                                    this.shapes[this.activeShape].rightTopX,
                                    this.shapes[this.activeShape].rightTopY,
                                    this.shapes[this.activeShape].bottomRightX,
                                    this.shapes[this.activeShape].bottomRightY,
                                    this.shapes[i].rightTopX,
                                    this.shapes[i].rightTopY,
                                    this.shapes[i].bottomRightX,
                                    this.shapes[i].bottomRightY)
                                ||  this.intersect(
                                    this.shapes[this.activeShape].rightTopX,
                                    this.shapes[this.activeShape].rightTopY,
                                    this.shapes[this.activeShape].bottomRightX,
                                    this.shapes[this.activeShape].bottomRightY,
                                    this.shapes[i].bottomRightX,
                                    this.shapes[i].bottomRightY,
                                    this.shapes[i].bottomLeftX,
                                    this.shapes[i].bottomLeftY  //right-line
                                )
                                || this.intersect(
                                    this.shapes[this.activeShape].bottomLeftX,
                                    this.shapes[this.activeShape].bottomLeftY,
                                    this.shapes[this.activeShape].bottomRightX,
                                    this.shapes[this.activeShape].bottomRightY,
                                    this.shapes[i].leftTopX,
                                    this.shapes[i].leftTopY,
                                    this.shapes[i].bottomLeftX,
                                    this.shapes[i].bottomLeftY)
                                || this.intersect(
                                    this.shapes[this.activeShape].bottomLeftX,
                                    this.shapes[this.activeShape].bottomLeftY,
                                    this.shapes[this.activeShape].bottomRightX,
                                    this.shapes[this.activeShape].bottomRightY,
                                    this.shapes[i].leftTopX,
                                    this.shapes[i].leftTopY,
                                    this.shapes[i].rightTopX,
                                    this.shapes[i].rightTopY)
                                ||this.intersect(
                                    this.shapes[this.activeShape].bottomLeftX,
                                    this.shapes[this.activeShape].bottomLeftY,
                                    this.shapes[this.activeShape].bottomRightX,
                                    this.shapes[this.activeShape].bottomRightY,
                                    this.shapes[i].rightTopX,
                                    this.shapes[i].rightTopY,
                                    this.shapes[i].bottomRightX,
                                    this.shapes[i].bottomRightY)
                                ||  this.intersect(
                                    this.shapes[this.activeShape].bottomLeftX,
                                    this.shapes[this.activeShape].bottomLeftY,
                                    this.shapes[this.activeShape].bottomRightX,
                                    this.shapes[this.activeShape].bottomRightY,
                                    this.shapes[i].bottomRightX,
                                    this.shapes[i].bottomRightY,
                                    this.shapes[i].bottomLeftX,
                                    this.shapes[i].bottomLeftY  //bottom-line
                            ))
                            { this.over = true
                                this.shape(
                                    this.shapes[this.activeShape].leftTopX,
                                    this.shapes[this.activeShape].leftTopY,
                                    this.shapes[this.activeShape].rightTopX,
                                    this.shapes[this.activeShape].rightTopY,
                                    this.shapes[this.activeShape].bottomRightX,
                                    this.shapes[this.activeShape].bottomRightY,
                                    this.shapes[this.activeShape].bottomLeftX,
                                    this.shapes[this.activeShape].bottomLeftY,
                                    true,
                                    this.over)
                                requestAnimationFrame(this.allshapes);
                                break
                            }
                                    this.over = false
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
                                        true,
                                        this.over)
                                requestAnimationFrame(this.allshapes);
                            }}
                } else {
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
                        this.shapes[this.activeShape].active,
                        this.shapes[this.activeShape].over,
                        )
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
                this.shapes[this.activeShape].over = this.over
                this.allshapes()
                this.canvas.onmousedown = null
                this.canvas.onmousemove = null
            },
            intersect(x1,y1,x2,y2,x3,y3,x4,y4) {
                let det, gamma, lambda;
                det = (x2 - x1) * (y4 - y3) - (x4 - x3) * (y2 - y1);
                if (det === 0) {
                    return false;
                } else {
                    lambda = ((y4 - y3) * (x4 - x1) + (x3 - x4) * (y4 - y1)) / det;
                    gamma = ((y1 - y2) * (x4 - x1) + (x2 - x1) * (y4 - y1)) / det;
                    return (0 < lambda && lambda < 1) && (0 < gamma && gamma < 1);
                }
            }
        },
        mounted() {
            this.canvas = this.$refs.canvasArea
            this.context = this.canvas.getContext("2d")
            this.windowWidth = window.innerWidth;
            this.windowHeight = window.innerHeight;

        },
        updated() {
            this.allshapes()
        }
    }
</script>

<style scoped>
    .canvas {
        display: block;
        border: 1px solid black;

    }


</style>
