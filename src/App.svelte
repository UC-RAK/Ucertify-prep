<script>
	import * as myjson from "./question.json";
	import { fly } from "svelte/transition";
	import { tweened } from 'svelte/motion';
	import Modal from "./Modal.svelte";

	let questions = myjson.default;
	console.log(questions[0].question);
	
	
	let page = "overview";
	let modal;



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
// function answerClick(){
// 	answers[questionPointer]=i;
// 	attamp++;
// }
	
 let answers = new Array(questions.length).fill(null);
 let questionPointer = -1;
 let i=0;
 let incorrect = 0;

 function getScore(){
   let score = answers.reduce((acc, val, index)=>{
    if(questions[index].correctIndex == val){
	 return acc + 1;
  }
     return acc
	},0)
	return score;
	
}

 
 function restartQuiz(){
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
	  let ele = document.getElementsByClassName("butn");
	  for(let i=0; i<ele.length; i++){
		 
		
		  ele[i].checked = false;
		  
		  
	  }
  }
 
  function endBtn(){
	  alert("Are you sure to submit this test");
    questionPointer = 12;
  }
//   function nextBtn(){
// 	  if(questionPointer < questions.length){
// 		  document.getElementById("next").disabled = false;
// 	  }
// 	  else{
// 		  document.getElementById("next").disabled = true;
// 	  }
//   }

let b = [];
	b = Array.from(questions.keys());
	console.log(b);
	

let selectedquestion;
function show(x){
	console.log(b);
	console.log(x);
	page = "details";
    selectedquestion = b.find(i => i == x);
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

</script>

<style>
  .app {
	  position : absolute;
	  top: 0px;
	  left: 0px;
	  width: 100vw;
	  height: 100vh;
  }
  .app > div{
	  width: 100%;
	  height: 100%;

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
	  padding: 50px;
	  padding-top: 0px;
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
	 margin: 0 1rem;
  }
  
 
 
 .bottom {
	 margin-left: 40%;
	 margin-right: 6%;
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
 }
 .show{

	  background: #4a77dc;
	  color: white;
	  border: none;
	  outline: none;
	  border-radius: 10px;
	  cursor: pointer;
 }
 ul{
	 list-style-type:none;
 }
 #options{
	 width: 80%;
	 
 }
 .mins{
	 color: black;
 }
 .secs{
	 color: black;
 }
 #para{
	 margin-top:0;
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
  /* .review{
	  width:50%;
	  height:70%;
	  overflow: auto;
	  margin-top:1rem;
	  border: 1px solid black;
	  padding: 1rem;
	 
	 
  } */
  .flex-container {
	  display: flex;
  }
  .flex-child{
	  flex:1;
	  border: 1px solid black;
	  margin: 2px;
	  padding: 10px;
  }
  .flex-child:first-child{
	  margin-right: 10px;
	  width: 30%;
  }
  .question {
	  width: 100%;
	  height: 40%;
	  border: 1px solid black;
	  margin: 1rem;
	  padding: 1rem;
  }
  .explanation{
	  width: 100%;
	  height: 50%;
	  margin: 1rem;
	  padding: 1rem;
	  border: 1px solid black;
  }
  .btn1{
	  float: left;
	  margin: 0.5rem;
	  margin-top: 4rem;
  }
  #btn2{
	  margin-left: 10rem;
  }
  
</style>
{#if page === 'overview'}
<div class="app">
	{#if questionPointer==-1}
	<p class="upper">uCertify Test prep</p>
	
	<div class="start-screen">
		<button on:click={()=>questionPointer = 0}>
			Start Quiz
		</button>
	</div>
	{:else if !(questionPointer > answers.length-1)}
	 <div class="quiz-screen">
		<p class="upper">uCertify Test prep</p>
     <div class="main">
	{#if showDiv}
	  <div transition:fly = {{ x:0, y:200}} id="myDIV">
		<ul>
		{#each questions as question,i}
	   <li on:click={() =>{list(i)}}><b>Q{i+1}:-</b>{question.question}</li>
	   {/each}
	</ul>
	 </div>
	  {/if}
	  <div transition:fly = {{ x:0, y:0}} id="options">
      <h2>
		 {questions[questionPointer].question}
	  </h2>
	 
       {#each questions[questionPointer].options as opt,i}
	 
	   
  <div class="form-check">
   <ul list-style-type =  none>
	   <li>
    <input  class="form-check-input butn" type="radio" name="flexRadioDefault" id="flexRadioDefault1" on:click={()=>{answers[questionPointer]=i, attamp++}}>
    {opt}</li>
     <!-- <label class="form-check-label" for="flexRadioDefault1">{opt}</label> -->
    </ul>
      </div>
	   {/each}
	  </div>
	 </div>
	 <div class="footer">
		 <p class="bottom">
			<span class="mins">{minutes}</span>{minname}: <span class="secs">{seconds}</span>s  
		 <button class="show" on:click={showbox}>List</button>
		 {questionPointer+1}/{questions.length}
			<button class="show" disabled={questionPointer==0} on:click={()=>{questionPointer--}}>
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
	<p id = "para" class="upper">uCertify Test prep</p>
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
            <h2> {questions.length-attamp}<br>UnAttemp:</h2>
		</div>
		
		<!-- <button on:click={restartQuiz}>
			Restart Quiz
		</button> -->
		<!-- <button >Lessons</button> -->
		<div class="flex-container">
		<div class="flex-child">
           <p>View All chapter</p>
		   <p>Following process and commnication</p>
		</div>
		<div class = "flex-child">
			<ul id='container'>
				{#each questions as question,x}
			  <li on:click={() =>{show(x)}}>
				<b>Q{x+1}:-</b>{question.question}>
			</li>
			   {/each}
			</ul>
		</div>
	</div>
	</div>
	{/if}

	</div>
	{/if}
	{#if page === 'details'}
	<p id = "para" class="upper">uCertify Test prep</p>
	<section>
		<h1 style="padding=1rem">welcome to review page</h1>
		<div class="question">
			<h1>Questions</h1>
		   <p><b>Q{selectedquestion + 1}:- </b>{questions[selectedquestion].question}</p>
		  </div>
		<div class="explanation">
			<h1>Explanations</h1>
		  <p>{@html questions[selectedquestion].explanation}</p>
		</div>
		<button on:click={Back}>Go to result page</button>
	</section>
	{/if}
  