<template>
    <div class="calculator">
        <div class="display">{{ current || '0'}}</div>
        <div class="btn" @click="clear">C</div>
        <div class="btn" @click="sign">+/-</div>
        <div class="btn" @click="percent">%</div>
        <div class="btn operator" @click="divide">/</div>
        <div class="btn" @click="append('7')">7</div>
        <div class="btn" @click="append('8')">8</div>
        <div class="btn" @click="append('9')">9</div>
        <div class="btn operator" @click="times">X</div>
        <div class="btn" @click="append('4')">4</div>
        <div class="btn" @click="append('5')">5</div>
        <div class="btn" @click="append('6')">6</div>
        <div class="btn operator" @click="minus">-</div>
        <div class="btn" @click="append('1')">1</div>
        <div class="btn" @click="append('2')">2</div>
        <div class="btn" @click="append('3')">3</div>
        <div class="btn operator" @click="add">+</div>
        <div class="zero btn" @click="append('0')">0</div>
        <div class="btn" @click="dot()">.</div>
        <div class="btn operator" @click="equal()">=</div>

    </div>
</template>

<script>
    export default {
        data(){
            return {
                previous: null,
                current: '',
                operator: '',
                operatorClicked: false,
            }
        },
        methods: {
            clear(){
                this.current = '';
            },
            sign(){
                var current = this.current;
                if(current != 0){
                    this.current = current.charAt(0) === '-' ? current.slice(1) : `-${current}`;
                }
            },
            percent(){
                this.current = `${parseFloat(this.current) / 100}`;
            },
            append(num){
                if(this.operatorClicked){
                    this.current = '';
                    this.operatorClicked = false;
                }
                if(!(num == '0' && this.current.indexOf('0') == 0)){
                    if(this.current.charAt(0) == '0' && this.current > 0){
                        this.current = `${this.current.slice(1)}${num}`;
                    }else{
                        this.current = `${this.current}${num}`;
                    }
                }else{
                    if(parseFloat(this.current) > 0){
                        this.current = `${this.current}${num}`;
                    }
                }

            },
            dot(){
                if(this.current.indexOf('.') === -1){
                    this.append('.');
                }
            },
            divide(){
                this.operator = (a,b) => a / b;
                this.onOperatorClicked();
            },
            minus(){
                this.operator = (a,b) => a - b;
                this.onOperatorClicked();
            },
            times(){
                this.operator = (a,b) => a * b;
                this.onOperatorClicked();
            },
            add(){
                this.operator = (a,b) => a + b;
                this.onOperatorClicked();
            },
            equal(){
                this.current = `${this.operator(parseFloat(this.previous), parseFloat(this.current))}`;
                this.previous = '';
            },
            onOperatorClicked(){
                this.previous = this.current;
                this.operatorClicked = true;
            }
        }
    }
</script>
<style scoped>
    .calculator {
        width: 400px;
        margin: 0 auto;
        font-size: 40px;
        display: grid;
        grid-template-columns: repeat(4, 1fr);
        grid-auto-rows: minmax(50px, auto);
    }

    .display {
        grid-column: 1/5;
        background-color: aquamarine;
        overflow: auto;
    }

    .zero {
        grid-column: 1/3;
    }

    .btn{
        background-color: #eee;
        border: 1px solid #999;
    }

    .operator{
        background-color: orange;
        color: white;
    }
</style>
