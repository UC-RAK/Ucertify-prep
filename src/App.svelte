<script>
	import * as myjson from "./question.json";
	import { fly } from "svelte/transition";
	import { tweened } from 'svelte/motion';
	import Modal from "./Modal.svelte";
	

	let questions = myjson.default;
	
	
	let page = "overview";
	let modal;

    let checkanswer = {

	};


	let original = 5 * 60;
	let timer = tweened(original);
	setInterval(()=>{
		if ($timer > 0) $timer--;
	}, 1000);

	$: minutes = Math.floor($timer / 60);
	$: minname = minutes > 1 ? "mins" : "min";
	$: seconds = Math.floor($timer - minutes * 60);
	

let attamp = 0;
let correct = 0;

 let answers = new Array(questions.length).fill(null);
 let questionPointer = -1;
 let i=0;
 let incorrect = 0;
 let score;

 function getScore(){
   let score = answers.reduce((acc, val, index)=>{
    if(questions[index].correctIndex === val){
	 return acc + 1;
  }
    return acc
	},0)
	return score;
	
}

 
 function restartQuiz(){
	 page = "overview";
   answers = new Array(questions.length).fill(null);
   questionPointer = 0;
 }
 let showDiv = false;
 let showElement = false;
 function showbox(){
	showDiv = !showDiv;
	showElement =!showElement;
	if(showElement){
	document.getElementById("options").style.float = "right";
	}
	else{
		document.getElementById("options").style.float = "left";
	}
 }
  function incrementquestion(){
	  questionPointer++;
	  console.log(questionPointer);
	  let ele = document.getElementsByClassName("butn");
	  console.log(ele.length);
	  for(let i=0; i<ele.length; i++){
		if(checkanswer[questionPointer] != undefined && checkanswer[questionPointer][i] == "on"){
		   ele[i].checked = true;
	   }
	   else{
		ele[i].checked = false;
	   }
	  }
  }
   
   function previousquestion(){
	   questionPointer--;
	   unattamp++;
	   let ele = document.getElementsByName("flexRadioDefault");
	   for(let i=0;i<ele.length; i++){
	   if(checkanswer[questionPointer][i] == "on"){
		   ele[i].checked = true;
	   }
   }
}
  function endBtn(){
    questionPointer = 12;
  }

let keys = [];
	keys = Array.from(questions.keys());
	console.log(keys);
	

let selectedquestion;
function show(x){
	page = "details";
    selectedquestion = keys.find(i => i == x);
	console.log(selectedquestion);
	
	return selectedquestion;

}
 function list(i){
	 questionPointer = i;
 }

 function Back(){
	 page = 'overview';
	 questionPointer = 12;
 }
 console.log(questions[0].correctIndex);
 console.log(questions.length);

 function  nextMove(){
	 selectedquestion++;
 }
 function previousMove(){
	 selectedquestion--;
 }
 let unattamp = 11;
 function checkAnswer(i){
	answers[questionPointer] = i
		
		attamp++;
	    unattamp--;
        var ele = document.getElementsByName("flexRadioDefault");
		for( let i=0; i < ele.length; i++) {
			if(ele[i].checked){
			checkanswer[questionPointer] = {
				 [i] : ele[i].value
							  }
					}
				}
				console.log(answers);
   }
		
</script>

<style>
	
  .app {
	  position : absolute;
	  top: 0px;
	  left: 0px;
	  width: 98vw;
	  height: 100vh;
  }
  .app > div{
	  width: 100%;
	  height: 80%;

  }
  .app .start-screen{
	  display: flex;
	  justify-content: center;
	  align-items: center;
  }
  .app .start-screen button,
  .app .start-screen button{
	  padding: 10px 20px;
	  background: #4a77dc;
	  color: white;
	  border: none;
	  outline: none;
	  border-radius: 20px;
	  cursor: pointer;
  }
  .app .quiz-screen .main {
	  width: 93%;
	  margin-right:0;
	  margin-left: 1rem;
	  padding-top: 0px;
  }
  
  #options{
	  margin-right: 0px;
  }
 
  .quiz-screen{
	  display:inline-block;
  }
  .app .quiz-screen .footer{
	  position: fixed;
	  bottom: 0px;
	  left: 0px;
	  width: 100%;
	  height: 60px;
	  display: flex;
	  justify-content: space-between;
	  align-items: center;
	  background: #eee;
  }
  .app .quiz-screen .footer {
	 margin-top:0px;
  }
  .footer {
     margin-right: 1rem;
	 width: 97%;
  }
  
 
 .bottom {
	 margin-left: 26%;
	 margin-right: 7%;
 }
 .upper {
	 padding-top: 2px;
	 background:#DA0037;
	 text-align: center;
	 color: white;
	 height: 4rem;
	 font-weight: bold;
	 font-size: 1.5rem;
	 width: 100%;
 }
 .form-check{
	 width: 30%;
	 margin: 1rem;

 }
 #myDIV{
	 display:inline-block;
	 width: 18%;
	 height: 23rem;
	 overflow: scroll;
	 margin-top:6rem;
 }
 .show{

	  background: #4a77dc;
	  color: white;
	  border: none;
	  outline: none;
	  border-radius: 10px;
	  cursor: pointer;
	  margin-right: 1rem;
	  width: 6rem;
 }
 ul{
	 list-style-type:none;
 }
 #options{
	 width: 80%;
	 margin-top: 6rem;
	 }
 .mins{
	 color: black;
 }
 .secs{
	 color: black;
 }
 #para{
	 margin-top:0;
	 width:100%;
 }
 .score-screen{
	 width: 100%;
	 height: 100%;
	 margin: 0 10px;
 }
 .score{
	 border: 1px solid black;
	 display:inline-block;
	 width: 13rem;
	 height: 7rem;
	 padding: 2px;
	 margin: 0.5rem;
	 margin-top:5rem;
 }
 .score h2{
	 padding: 0.5rem;
	 text-align: center;
 }
 #div1{
	 text-align:center;
 }
 #first{
	 margin-left: 10%;
 }
  
  .flex-container {
	  display: flex;
	  margin-left:0px;
	  width:98%;
  }
  .flex-child{
	  flex:1;
	  border: 1px solid black;
	  margin: 1px;
	  padding: 5px;
  }
  .flex-child:first-child{
	  margin-right: 5px;
	  width: 30%;
  }
  .question {
	  width: 97%;
	  height: 40%;
	  border: 1px solid black;
	  margin: 1rem;
	  padding: 1rem;
	  border-radius: 10px;
	  background-color:#F8F8F8;
  }
  .explanation{
	  width: 97%;
	  height: 50%;
	  margin: 1rem;
	  padding: 1rem;
	  border: 1px solid black;
	  border-radius: 10px;
	  background-color: #F8F8F8;
  }
  .btn1{
	  float: left;
	  margin: 0.5rem;
	  margin-top: 4rem;
  }
  #btn2{
	  margin-left: 10rem;
  }
  #back{
	  margin-left: 30%;
  }
  .green{
	  color: green;
  }
  .flex-child ul li:hover{
	  cursor: pointer;
  }
  .show-reviewbtn{
	background: #4a77dc;
	  color: white;
	  border: none;
	  outline: none;
	  border-radius: 10px;
	  cursor: pointer;
	  margin-right: 1rem;
	  width: 8rem; 
	  margin-bottom:2rem;
	  margin-top:1.5rem;
  }
  .blue{
	  color: blue;
  }
</style>
{#if page === 'overview'}
<p class="upper">uCertify Test prep</p>
<div class="app">
	{#if questionPointer==-1}
	
	
	<div class="start-screen">
		<button on:click={()=>questionPointer = 0}>
			Start Quiz
		</button>
	</div>
	{:else if !(questionPointer > answers.length-1)}
	 <div class="quiz-screen">
		
     <div class="main">
		
	{#if showDiv}
	  <div transition:fly = {{ x:0, y:200}} id="myDIV">
		<ul id="listItem">
		{#each questions as question,i}
	   <li class="{(checkanswer[i] != undefined ) ?'green': 'navigate'}" on:click={() =>{list(i)}}><b>Q{i+1}:-</b>{question.snippet}</li>
	   {/each}
	</ul>
	 </div>
	  {/if}
	  <div transition:fly = {{ x:0, y:0}} id="options">
      <h2>
		 <b>Q{questionPointer+1}:-</b>{questions[questionPointer].question}
	  </h2>
	 
       {#each questions[questionPointer].options as opt,i}   
  <div class="form-check">
   <ul list-style-type =  none>
	   <label>
    <input  class="form-check-input butn" type="radio" name="flexRadioDefault" id="radiobutn" on:click={() => {checkAnswer(i)}}>
    {opt}</label>
    
    </ul>
      </div>
	   {/each}
	  </div>
	 </div>
	 <div class="footer">
		 <p class="bottom">
			<span class="mins">{minutes}</span>{minname}: <span class="secs">{seconds}</span>s  
		 <button class="show" on:click={showbox}>List</button>
		 {questionPointer+1}/{questions.length}&nbsp;&nbsp;
			<button class="show" disabled={questionPointer==0} on:click={previousquestion}>
				Previous
			   </button>
			   <button id="next" class="show" disabled={questionPointer >= questions.length-1} on:click={incrementquestion}>
				Next
			   </button>
			   <button id="end" class="show" on:click={() => modal.show()}>
			    End
			</button>
			</p>
			<Modal bind:this={modal} >
				<h2 style="margin-top: 2rem">Are you sure to end this test</h2>
				<button id="btn2" class="btn1" on:click={() => modal.hide()}>Cancel</button>
				<button class="btn1" on:click={endBtn}>Yes sure continue..</button>
			</Modal>
	 </div>
	</div>
	{:else}
	<!-- <p class="upper">uCertify Test prep</p> -->
		<div class="score-screen"> 
		<p id = "div1">
			<div id="first" class="score">
				<h2>
					{getScore()}/{questions.length}<br>Your Score: 
				</h2>
			</div>
			<div class="score">
				<h2>
					{questions.length}<br>Items: 
				</h2>
			</div>
		<div class="score">
            <h2> {getScore()}<br>Correct:</h2>
		</div>
		<div class="score">
            <h2> { questions.length - getScore()}<br>InCorrect:</h2>
		</div>
		<div class="score">
            <h2> {unattamp}<br>UnAttemp:</h2>
		</div>
		
		<!-- <button on:click={restartQuiz}>
			Restart Quiz
		</button> -->
		
		<div class="flex-container">
		<div class="flex-child">
           <p>View All chapter</p>
		   <p>Following process and commnication</p>
		</div>
		<div class = "flex-child">
			<ul id='container'>
				{#each questions as question,x}
			  <li on:click={() =>{show(x)}}>
				<b>Q{x+1}:-</b>{question.question}
			  </li>
			<br>
			   {/each}
			</ul>
		</div>
		
	</div>
	</div>
	{/if}

	</div>
	{/if}
	{#if page === 'details'}
	
	<section>
		<p id = "para" class="upper">uCertify Test prep</p>
		<h1 style="text-align : center;">welcome to review page</h1>
		
		<div class="question">
			<h1>Questions</h1>
		   <p><b>Q{selectedquestion + 1}:- </b>{questions[selectedquestion].question}</p>
		{#each questions[selectedquestion].options as opt,i}
		<div class="form-check">
		 <ul list-style-type =  none>
			 <li class={(checkanswer[selectedquestion] != undefined &&  checkanswer[selectedquestion][i] == 'on') ? "blue" : ""}>
		  <input  disabled class="form-check-input butn" type="radio" name="radio-btn" id="flexRadioDefault1">
		  {opt}</li>
		  
		  </ul>
			</div>
			 {/each}
			</div>
			 <div class="explanation">
				<h1>Explanations</h1>
				<p>{@html questions[selectedquestion].explanation}</p>
			</div>
		<p>
		<button class="show-reviewbtn"  id="back" on:click={previousMove}>Previous</button>
		<button class="show-reviewbtn"  on:click="{nextMove}">Next</button>
		<button class="show-reviewbtn"  on:click="{Back}">Go to Result</button>
		<button class="show-reviewbtn"  on:click={restartQuiz}>
			Restart Quiz
		</button>
	</p>	

	</section>
	{/if}
  