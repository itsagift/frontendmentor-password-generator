<script>
	import StrengthMeter from "./StrengthMeter.svelte";
	export let password = "P4$5W0rD!";
	export let characters = '';
	export let passwordLength = 0;
	export let isUpperCase = false;
	export let isLowerCase = false;
	export let isSymbols = false;
	export let isNumeric = false;

	export let options = {
		"isUpperCase": false,
		"isLowerCase": false,
		"isSymbols": false,
		"isNumeric": false
	}
	export let strength = 0;

	function handleSubmit() {
		console.log("Lowercase", isLowerCase, "uppercase", isUpperCase);
	}

	function changeTest(){
		let selectedBoxes = Object.values(options).filter((item) => item).length
		strength = selectedBoxes;
		console.log("changing!", selectedBoxes)
	}

	function getRandomInteger (min, max) {
    return Math.floor(Math.random() * (max - min + 1)) + min;
	}

	function setUpperCase() {
    if (options.isUpperCase) {
        characters += 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
				console.log(characters)
    }
    return '';
	}
	function setLowerCase() {
    if (options.isLowerCase) {
        characters += 'abcdefghijklmnopqrstuvwxyz';
    }
    return '';
	}
	function setSymbols() {
    if (options.isSymbols) {
        characters += '!@#$%^&*()<>,.?/[]{}-=_+|/';
    }
    return '';
	}
	function setNumber () {
    if (options.isNumeric) {
        characters += '0123456789';
    }
    return '';
	}

	function passwordCharacters () {
		console.log("trying", characters)
    let newPassword = '';
    if (characters.length > 0) {
			console.log("new", passwordLength)
        for (let i = 0; i < passwordLength; i++) {
            newPassword += characters[getRandomInteger(0, characters.length - 1)];
						
        }
        characters = '';
        passwordLength = 0;
        return newPassword;
    }
	}

	function generatePassword () {
    setUpperCase(options.isUpperCase);
    setLowerCase(options.isLowerCase);
    setSymbols(options.isSymbols);
    setNumber(options.isNumeric);
    
    let test = passwordCharacters();
		password = test;
	}

	
</script>

<main>
	<h1>Password Generator</h1>
	<div class="password-display">{password}</div>
	<div class="generator-container">

		<div class="length-slider">
			<div class="slider-top">
				<div class="slider-top-title">Character Length</div>
				<div class="slider-top-length">{passwordLength}</div>
			</div>
			<div class="slider-bottom">
				<input type="range" min="0" max="20" bind:value={passwordLength}/>
			</div>
		</div>

		<form on:submit|preventDefault={generatePassword} on:change={changeTest}>
		<ul class="checkboxes">
			<li class="checkbox-item">
				<input id="uppercase" type="checkbox" bind:checked={options.isUpperCase}/>
				<label for="uppercase">Upper Case</label>
			</li>
			<li class="checkbox-item">
				<input id="lowercase" type="checkbox" bind:checked={options.isLowerCase}/>
				<label for="lowercase">Lower Case</label>
			</li>
			<li class="checkbox-item">
				<input id="symbols" type="checkbox" bind:checked={options.isSymbols}/>
				<label for="symbols">Symbols</label>
			</li>
			<li class="checkbox-item">
				<input id="numeric" type="checkbox" bind:checked={options.isNumeric}/>
				<label for="numeric">Numeric</label>
			</li>
		</ul>

		<StrengthMeter strengthTest={strength}/>

		<button type=submit>
			Submit
		</button>

		</form>
	</div>
	
</main>

<style>
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
		
	}

	h1 {
		text-transform: uppercase;
	}

	.generator-container{
		width: 50%;
		margin: 0 auto;
		background-color: #ddd;
		padding: 30px;
	}

	.length-slider{
		padding: 10px;
	}

	.slider-top{
		display: flex;
		justify-content: space-between;
	}

	.checkboxes{
		list-style: none;
		padding: 0;
	}

	.checkbox-item{
		display: flex;
		align-items: center;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>