<script>
	import Slide from './Slide.svelte'
	import { words } from './content-day1'

	let pos = 0
	let w = 0

	const handleKeydown = (event) => {
		switch(event.key){
			case 'ArrowRight': pos = pos < words.length-1 ? pos +1 : 0; break;
			case 'ArrowLeft': pos = pos > 0 ? pos -1 : words.length -1; break;
			case 'f': document.documentElement.requestFullscreen(); break;
		}
		if(!isNaN(event.key) && event.key <= words.length ){
			pos = parseInt(event.key) 
		}
	}

	let show = true
	$: {
		pos //the reactive declaration is fired every time pos changes
		w = (100 / (words.length-1)) * pos //progress
		show = false
		//if there is a background image in this words[pos] object, set it up..
		setTimeout( () => show = true, 40)
	}

	const roll = setInterval(()=>{pos++;if(pos > words.length-1) pos = 0}, 10000)
</script>

<svelte:window on:keydown={handleKeydown}/>
<svelte:head><title>Digitalt Teknikfag 2020</title></svelte:head>
<header style='width:{w}%'><title>Simon</title></header>
<main>
	{#if show}
		<Slide content={words[pos]} end={words.length -1 == pos ? true : false}/>
	{:else}
		<p />
	{/if}
</main>


<style>
	:global(body, html){
		margin:0;
		padding:0;
		box-sizing: border-box;		
	}
	header{
		height:2vh;
		position:fixed;
		background: #ff3e00;
		transition:all 1s ease;
		width: 1vw;
	}
	main {
		display:grid;
		height:100vh;
		place-items:center;
	}
</style>