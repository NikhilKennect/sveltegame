<script>
// @ts-nocheck
	import { confetti } from '@neoconfetti/svelte';
	import statements from './statements';

	import './styles.css';
	import Key from "./Key.svelte";
	let keys = [['Q','W','E','R','T','Y','U','I',"O","P"],['A','S','D','F','G','H','J','K',"L"],['Z','X','C','V','B','N','M']]
	let accptKeys = ['Q','W','E','R','T','Y','U','I',"O","P",'A','S','D','F','G','H','J','K',"L",'Z','X','C','V','B','N','M'];
	let comms = ['BACKSPACE','ENTER','SPACE']
	let index = 0;
	
	$:currState = statements[index]

	
	let keyPress = false;
	let currKey = null;
	let currStrng = "";
	let score = 0;
	let won = false;
	let lap = 0;	
	let strtTime = 0;
	let startBool = false;



	function checkWin(){
		if(currStrng.length==1 && !startBool){
			startBool = true
			strtTime = new Date().getTime();
		}
		if(currState === currStrng){
			score += 100;
			index = (index+1) % statements.length;
			let end = new Date().getTime();
			lap =  Math.min(lap,((end - strtTime)/1000).toFixed(0));
			startBool = false;
			return true
		};
		return false;
	}


	function hanKeyDown(e){
		if(true){
			if(accptKeys.includes(e.key.toUpperCase())){
				currKey = e.key.toUpperCase()
				currStrng += e.key
				if(checkWin()){
					currStrng = ""
					won = true;
					setTimeout(()=>{
						won = false;
					},1000)
				}
			}
			if(comms.includes(e.code.toUpperCase())){
				currKey = e.code.toUpperCase()
				if(currKey == 'SPACE'){
					currStrng += " ";
				}
				if(currKey == 'BACKSPACE'){
					currStrng = currStrng.slice(0,currStrng.length-1)
				}
			}
			keyPress = true
		}
	}
	const hanKeyUp = (e)=>{
			keyPress = false
			currKey = null
	}


</script>

<svelte:head>
	<title>Typing Nightmare</title>
</svelte:head>

<svelte:body on:keydown={hanKeyDown} on:keyup={hanKeyUp}/>
<main>
		<div class="app">
			
			<div class="intro-app">
				<div class="intro-app-title">Type the following sentence</div>
				<div class="intro-data">{currState}</div>
			</div>
			<input value={currStrng} disabled class="to-type-cont" placeholder="Start typing"/>
			<div class="keyb-holder">
				<div class="row-1" >
						{#each keys[0] as k, i}
						<Key key={k} pressed={k==currKey && keyPress?true:false}/>
						{/each}
				</div>
				<div class="row-2" >
						{#each keys[1] as k, i}
						<Key key={k} pressed={k==currKey && keyPress?true:false}/>
						{/each}
				</div>
				<div class="row-3" >
						{#each keys[2] as k, i}
						<Key key={k} pressed={k==currKey && keyPress?true:false}/>
						{/each}
				</div>
				<div class="row-4" >
						<div class="space-key" class:pressed={currKey=='SPACE'}>Space</div>
				</div>
				
				<div class="score-cont">
						<div>
							<div class="score-lab">Score</div>
							<div class="score-cnt">{score} Points</div>
						</div>
							
						<div>
							<div class="score-lab">Fastest Lap</div>
							<div class="score-cnt">{lap} Sec</div>
						</div>
				</div>

			</div>
			
			
		</div>
		{#if won}
			<div
				style="position: absolute; left: 50%; top: 30%"
				use:confetti={{
					// particleCount:  0 ,
					force: 0.7,
					stageWidth: window.innerWidth,
					stageHeight: window.innerHeight,
					colors: ['#ff3e00', '#40b3ff', '#676778']
				}}
			/>
		{/if}
</main>

<style>


main{
	margin: 0;
	width: 100vw;
	height: 100vh;
	display: flex;
	align-items: center;
	justify-content: center;
	overflow: hidden;
	
}



.keyb-holder{
	display: flex;
	flex-direction: column;
	gap: 10px;
	
}


.row-1{
	display: flex ;
	flex-direction: row;
	gap: 10px;
}

.row-2{
	display: flex ;
	flex-direction: row;
	margin-left: 25px;
	gap: 10px;
}
.row-3{
	display: flex ;
	flex-direction: row;
	margin-left: 75px;
	gap: 10px;
}

.row-4{
	margin-left: 130px;
	padding-top: 6px;
	box-sizing: border-box;
}

.space-key{
	width: 300px;
    height: 50px;
    display: flex;
    background-color: #d0d0d0;
    font-family: 'Source Code Pro';
    color: #000;
    justify-content: center;
    align-items: center;
    font-size: 17px;
    border-radius: 6px;
    border-bottom-left-radius: 9px;
    border-bottom-right-radius: 9px;
    border-bottom: 3.5px solid #bdbdbd;
    border-top: 2px solid #f5f5f5;
}

.to-type-cont{
	margin-bottom: 22px;
	font-size: 22px;
	font-family: 'Source Code Pro';
	display: flex;
	width: 100%;
	height: 36px;
	flex-direction: row;
	padding-left: 12px;
	/* gap: 10px; */
}

.blinker{

	width: 2px;
	height: 26px;
	background-color: #757575;
    animation: blink 1s infinite; /* '1s' is the duration, 'infinite' repeats the animation indefinitely */	
}


@keyframes blink {
      0% { opacity: 0; }
      50% { opacity: 1; }
      100% { opacity: 0; }
}


.intro-app{
	margin-bottom: 22px;
	display: flex;
	flex-direction: column;
	gap: 7px;
	justify-content: center;
	align-items: center;
	border-bottom: 1px solid #e0e0e0;
	padding-bottom: 22px;
}

.intro-app-title{
	font-size: 17px;
}

.intro-data{
font-size: 20px;
font-weight: 600;
}


.pressed{
    background-color: #99999999;	
}

.score-cont{
	display: flex;
	flex-direction: row;
	justify-content: space-between;
	align-items: center;
	margin-top: 32px;
	border-top: 1px solid #e0e0e0;
	padding-top: 22px;
}
.score-lab{
	color: goldenrod;
	font-size: 17px;
	font-weight: 500;
}
.score-cnt{
	color: #000;
	font-size: 26px;
	font-weight: 700;
}

</style>
