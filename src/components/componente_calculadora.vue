<template>
	<div class="calculator pt-md-5">
		<div class="calculator-container">
			<div class="calculator-input-display">
				<p class="count-string" :class="{'hidden': operator == ''}">{{`${valorConta} ${operator} ${valorConta2}`}}</p>
				<!--input type="number" class="calculator-input-text" v-model="inputValue" @keydown.esc="limpar('tudo')" @keypress="detectKey"/-->
				<div class="calculator-input-text" @keydown.esc="limpar('tudo')" @keypress="detectKey">{{inputValue}}</div>
			</div>
			<div class="calculator-buttons">
				<div class="row g-0">
					<div class="col calculator-button-col">
						<button class="btn calculator-button" @click="setPercentage">%</button>
					</div>
					<div class="col calculator-button-col">
						<button class="btn calculator-button" @click="limpar('tudo')">AC</button>
					</div>
					<div class="col calculator-button-col">
						<button class="btn calculator-button" @click="limpar('campo')">CE</button>
					</div>
					<div class="col calculator-button-col">
						<button class="btn calculator-button" @click="this.setBackspace">&larr;</button>
					</div>
				</div>
				<div class="row g-0">
					<div class="col calculator-button-col">
						<button class="btn calculator-button" @click="setDivisionOne">1/x</button>
					</div>
					<div class="col calculator-button-col">
						<button class="btn calculator-button" @click="setPotenceByTwo">x²</button>
					</div>
					<div class="col calculator-button-col">
						<button class="btn calculator-button" @click="setRaizquadrada">²&radic;</button>
					</div>
					<div class="col calculator-button-col">
						<button class="btn calculator-button" @click="setDivision">/</button>
					</div>
				</div>
				<div class="row g-0">
					<div class="col calculator-button-col">
						<button class="btn calculator-button" @click="setInputVal(7)">7</button>
					</div>
					<div class="col calculator-button-col">
						<button class="btn calculator-button" @click="setInputVal(8)">8</button>
					</div>
					<div class="col calculator-button-col">
						<button class="btn calculator-button" @click="setInputVal(9)">9</button>
					</div>
					<div class="col calculator-button-col">
						<button class="btn calculator-button" @click="setMultiplication">X</button>
					</div>
				</div>
				<div class="row g-0">
					<div class="col calculator-button-col">
						<button class="btn calculator-button" @click="setInputVal(4)">4</button>
					</div>
					<div class="col calculator-button-col">
						<button class="btn calculator-button" @click="setInputVal(5)">5</button>
					</div>
					<div class="col calculator-button-col">
						<button class="btn calculator-button" @click="setInputVal(6)">6</button>
					</div>
					<div class="col calculator-button-col">
						<button class="btn calculator-button" @click="setSubtract">-</button>
					</div>
				</div>
				<div class="row g-0">
					<div class="col calculator-button-col">
						<button class="btn calculator-button" @click="setInputVal(1)">1</button>
					</div>
					<div class="col calculator-button-col">
						<button class="btn calculator-button" @click="setInputVal(2)">2</button>
					</div>
					<div class="col calculator-button-col">
						<button class="btn calculator-button" @click="setInputVal(3)">3</button>
					</div>
					<div class="col calculator-button-col">
						<button class="btn calculator-button" @click="setSum">+</button>
					</div>
				</div>
				<div class="row g-0">
					<div class="col calculator-button-col">
						<button class="btn calculator-button" @click="numInvert">+/-</button>
					</div>
					<div class="col calculator-button-col">
						<button class="btn calculator-button" @click="setInputVal(0)">0</button>
					</div>
					<div class="col calculator-button-col">
						<button class="btn calculator-button" @click="setInputVal('.')">.</button>
					</div>
					<div class="col calculator-button-col">
						<button class="btn calculator-button" @click="result">=</button>
					</div>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	data() {
		return{
			inputValue:0,
			valorConta:0,
			valorConta2:"",
			operator: ''
		}
	},
	watch: {
		inputValue(newVal,oldVal){
			console.log(oldVal, newVal);
			if(oldVal == 0){
				this.inputValue = newVal.toString()[0]
			}
			if (newVal.toString().length == 0) {
				this.inputValue = 0
			}
		}
	},
	mounted(){
		let self = this;
		window.addEventListener('keyup', (e)=>{
			self.detectKey(e)
		})
		
	},
	methods: {
		result(){
			console.log(this.valorConta, this.inputValue, this.operator);
			this.valorConta2 = this.inputValue
			if(this.valorConta != "" && this.inputValue != "" && this.operator != ""){
				if(this.operator == "raiz"){
					this.inputValue = Math.sqrt(this.valorConta);
				}else if(this.operator == "%"){
					this.inputValue = ((this.valorConta / 100) * this.inputValue);
				}else{
					this.inputValue = eval(this.valorConta + this.operator + this.inputValue);
				}
			}
		},
		detectKey(event){
			const keyName = event.key;
			console.log(event);
			console.log('keypress event \n \n' + 'chave:' + keyName);
			if(keyName == "+"){
				event.preventDefault()
				this.setSum()
			}else if(keyName == "-"){
				event.preventDefault()
				this.setSubtract()
			}else if(keyName == "*"){
				event.preventDefault()
				this.setMultiplication()
			}else if(keyName == "/"){
				event.preventDefault()
				this.setDivision()
			}else if(keyName == "%"){
				event.preventDefault()
				this.setPercentage()
			}else if(keyName == "Enter"){
				event.preventDefault();
				this.result()
			}else if(keyName >= 0 && keyName <= 9 || keyName == "."){
				this.setInputVal(keyName)
			}else if(keyName == "Backspace"){
				this.setBackspace()
			}else if(keyName == "Escape"){
				this.limpar('tudo')
			}
		},
		setInputVal(num){
			if(this.inputValue == 0){
				this.inputValue = num
			}else{
				this.inputValue = this.inputValue.toString().concat(num)
			}
			event.target.blur()
		},
		setSum(){
			this.valorConta2 = ''
			console.log("adição");
			if(this.operator != ''){
				this.valorConta = eval(this.valorConta + this.operator + this.inputValue);
			}else{
				this.valorConta = this.inputValue	
			}
			this.operator = "+"
			this.inputValue = 0
			event.target.blur()
		},
		setSubtract(){
			this.valorConta2 = ''
			console.log("subtração");
			if(this.operator != ''){
				this.valorConta = eval(this.valorConta + this.operator + this.inputValue);
			}else{
				this.valorConta = this.inputValue	
			}
			this.operator = "-"
			this.inputValue = 0
			event.target.blur()
		},
		setMultiplication(){
			this.valorConta2 = ''
			console.log("multiplicação");
			if(this.operator != ''){
				this.valorConta = eval(this.valorConta + this.operator + this.inputValue);
			}else{
				this.valorConta = this.inputValue	
			}
			this.operator = "*"
			this.inputValue = 0
			event.target.blur()
		},
		setDivision(){
			this.valorConta2 = ''
			console.log("divisão");
			if(this.operator != ''){
				this.valorConta = eval(this.valorConta + this.operator + this.inputValue);
			}else{
				this.valorConta = this.inputValue	
			}
			this.operator = "/"
			this.inputValue = 0
			event.target.blur()
		},
		setPercentage(){
			this.valorConta2 = ''
			console.log("porcentagem");
			if(this.operator != ''){
				this.valorConta = eval(this.valorConta + this.operator + this.inputValue);
			}else{
				this.valorConta = this.inputValue	
			}
			this.operator = "%"
			this.inputValue = 0
			event.target.blur()
		},
		setDivisionOne(){
			this.valorConta2 = ''
			this.valorConta = 1
			this.operator = "/"
			this.result()
		},
		setPotenceByTwo(){
			this.valorConta2 = ''
			this.valorConta = this.inputValue
			this.operator = "*"
			this.result()
		},
		setRaizquadrada(){
			this.valorConta2 = ''
			this.valorConta = this.inputValue
			this.operator = "raiz"
			this.result()
		},
		numInvert(){
			this.inputValue = (this.inputValue != 0) ? this.inputValue * -1 : 0 
		},
		setBackspace(){
			if(this.inputValue.toString().length > 1){
				this.inputValue = this.inputValue.toString().slice(0, -1);
			}else{
				this.inputValue = 0
			}
		},
		limpar(type){
			this.inputValue = 0
			if(type == "tudo"){
				this.valorConta = 0
				this.operator = ''
				this.valorConta2 = ''
			}
		},
	}
}
</script>

<style lang="scss">
.calculator-container{
	max-width: 500px;
	box-shadow: -1px 1px 20px 5px #ccc;
	margin: auto;	
	.calculator-input-display{
		padding-top: 10px;
		background: var(--background);


		.count-string{
			padding: 0;
			margin: 0;
			height: 30px;
		}
		.count-string.hidden{
			opacity: 0;
		}
		.calculator-input-text{
			height: 80px;
			width: 100%;
			font-size: 2.5em;
			text-align: end;
			overflow-x: auto;
		}
	}

	.calculator-buttons{
		height: 500px;
		padding: 10px;

		.calculator-button-col{
			padding: 5px;

			.calculator-button{
				width: 100%;
				height: 70px;
				border-radius: 0;
				font-size: 1.5em;
				background: var(--button-bg-color);
				color: var(--button-color);
			}
		}
	}
}

@media screen and (max-width: 767px) {
	.calculator-container{
		box-shadow: none !important;
		
		.calculator-buttons{
			height: calc(100vh - 120px) !important;

			.calculator-button-col{
				height: calc(13vh) !important;
				.calculator-button{
					height: -webkit-fill-available !important;
				}
			}
		}
	}
}
</style>