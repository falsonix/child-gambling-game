<script lang="ts">
  let wheel0: HTMLDivElement = $state();
  let wheel1: HTMLDivElement = $state();
  let wheel2: HTMLDivElement = $state();
  let wheels = $derived([wheel0, wheel1, wheel2]);
  
  const sleep = ms => new Promise(r => setTimeout(r, ms));

  const randomOf = <T>(list: Array<T>): [T, T[]] => {
    let selected = Math.floor(Math.random()*list.length)
    
    return [list[selected], list.filter((_, idx) => idx != selected)]
  }
  
  const spin = async () => {
    // navigator.serial.requestPort({ filters: [{}] })
    
    wheel0.classList.add("spin");
    await sleep(1000);
    
    wheel1.classList.add("spin");
    await sleep(1000);
    
    wheel2.classList.add("spin");
    await sleep(2000);
    
    wheels.forEach((x) => x.classList.remove("spin"));
    
    if (Math.random() > 0.7) {
      // We won!
      
      if (Math.random() > 0.7) {
        console.log("won barely")
        // ... but barely
        let [losingWheel, rest] = randomOf(wheels);
        
        losingWheel.classList.add("skull")
        rest.forEach((x) => x.classList.add("sparkles"))
      } else {
        console.log("won absolutely")
        // ... absolutely
        wheels.forEach((x) => x.classList.add("sparkles"))
      }
    } else {
      // We lost...
      
      if (Math.random() > 0.7) {
        console.log("lost absolutely")
        // ... absolutely
        wheels.forEach((x) => x.classList.add("skull"))
      } else {
        console.log("lost barely")
        // but barely
        let [winningWheel, rest] = randomOf(wheels);
        
        winningWheel.classList.add("sparkles")
        rest.forEach((x) => x.classList.add("skull"))
      }
    }
  }
</script>

<main>
    <h1>Underage Gambling Game</h1>
    <div class="wheel-container">
    <div class="wheel" bind:this={wheel0}>
        <div>💀</div>
        <div>✨</div>
        <div>💀</div>
        <div>✨</div>
        <div>💀</div>
    </div>
    <div class="wheel" bind:this={wheel1}>
        <div>💀</div>
        <div>✨</div>
        <div>💀</div>
        <div>✨</div>
        <div>💀</div>
    </div>
    <div class="wheel" bind:this={wheel2}>
        <div>💀</div>
        <div>✨</div>
        <div>💀</div>
        <div>✨</div>
        <div>💀</div>
    </div>
    </div>
    
    <button on:click={spin}>Spin</button>
</main>

<style>
    :global(*) {
        font-family: system-ui;
        box-sizing: border-box;
    }
    
    main {
        font-size: 2em;
        display: flex;
        flex-direction: column;
        gap: 2em;
        justify-content: center;
        align-items: center;
    }
    
    
    button {
        font-size: 2rem;
        padding: 1rem;
    }
    
    .wheel-container {
        font-size: 2em;
        display: flex;
        flex-direction: row;
    }
    
    .wheel {
        height: 6em;
        overflow-y: clip;
    }
    
    .wheel > div {
        height: 2em;
        width: 2em;
        border: 1px solid black;
        display: flex;
        justify-content: center;
        align-items: center;
    }
    
    :global(.spin > div) {
        animation: slots-anim 100ms infinite linear;
    }
    
    :global(.skull > div) {
        animation: slots-skull 50ms 1 forwards;
    }
    
    :global(.sparkles > div) {
        animation: slots-sparkles 100ms 1 forwards;
    }
    
    @keyframes slots-anim {
        from {
            transform: translateY(0);
        }
        
        to {
            transform: translateY(-4em);
        }
    }
    
    @keyframes slots-skull {
        from {
            transform: translateY(0);
        }
        
        to {
            transform: translateY(-2em);
        }
    }
    
    @keyframes slots-sparkles {
        from {
            transform: translateY(0);
        }
        
        to {
            transform: translateY(-4em);
        }
    }
</style>