<template>
	<!--img alt="Vue logo" src="./assets/logo.png"-->
	<div class="app-content" :class="theme">
		<div class="theme-container">
			<button class="btn btn-theme btn-theme-light" v-if="theme=='light'" @click="theme='dark'"><img class="icon-theme" src="./assets/sun-icon.png" alt=""></button> 
			<button class="btn btn-theme btn-theme-dark" v-if="theme=='dark'" @click="theme='light'"><img class="icon-theme" src="./assets/moon-icon.png" alt=""></button>
		</div>
		<ComponenteCalculadora/>
	</div>
</template>

<script>
import ComponenteCalculadora from './components/componente_calculadora.vue'

export default {
  name: 'App',
  components: {
	ComponenteCalculadora
  },
  data(){
	  return{
		  theme: "light"
	  }
  }
}
</script>

<style lang="scss">
:root{
	--backgound: #efefef;
	--color: #333;
	--button-bg-color: #fff;
	--button-color: #333;
}
.dark{
	--backgound: #333;
	--color: #fff;
	--button-bg-color: #000;
	--button-color: #fff;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;

	.app-content{
		background: var(--backgound);
		color: var(--color);
		height: 100vh;
	}

  .btn-theme{
	  position: absolute;
	  top: 10px;
	  left:10px;
	  background: #e5e5e5;
	  border-radius: 50%;
	  padding: 5px;
	  text-align: center;
  }

  .icon-theme{
	  width: 50px;
  }
}
</style>
