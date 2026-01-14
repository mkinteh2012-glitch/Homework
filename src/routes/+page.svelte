<script lang="ts">
    type Game = {
        id: string;
        name: string;
        description?: string;
        path: string;
    };

    let selectedGame: Game | null = null;

    let iframeElement: HTMLIFrameElement | null = null; // This links to the actual iframe in the HTML

    const games: Game[] = [
        {
            id: 'polytrack',
            name: 'PolyTrack',
            description: 'High-speed low-poly racing.',
            path: '/games/polytrack/index.html'
        }
        ,{
            id: 'bitlife',
            name: 'BitLife',
            description: 'A life simulation game.',
            path: '/games/bitlife/index.html'
        }
    ];

    function play(game: Game) {
        selectedGame = game;
    }

    function exit(): void {
        selectedGame = null;
    }

    function goFullscreen(): void {
        if (iframeElement) {
            if (iframeElement.requestFullscreen) {
                iframeElement.requestFullscreen();
            } else if ((iframeElement as any).webkitRequestFullscreen) { /* Safari */
                (iframeElement as any).webkitRequestFullscreen();
            } else if ((iframeElement as any).msRequestFullscreen) { /* IE11 */
                (iframeElement as any).msRequestFullscreen();
            }
        }
    }

</script>

<main>
    {#if !selectedGame}
        <div class="hero">
            <h1>GAMING</h1>
            <p>Select a game to start playing</p>
        </div>

        <div class="grid">
            {#each games as game}
                <button class="card" on:click={() => play(game)}>
                    <div class="card-content">
                        <h2><span class="icon">{#if game.id === 'bitlife'}👤{:else}🏎️{/if}</span>{game.name}</h2>
                        <p>{game.description}</p>
                    </div>
                </button>
            {/each}
        </div>
    {:else}
        <div class="player-container">
            <div class="controls">
                <button class="btn-secondary" on:click={exit}>← Back</button>
                <span class="game-title">{selectedGame.name}</span>
                <button class="btn-primary" on:click={goFullscreen}>⛶ Fullscreen</button>
            </div>
            
            <iframe 
                bind:this={iframeElement} 
                src={selectedGame.path} 
                title="Game Window"
                allow="fullscreen"
            ></iframe>
        </div>
    {/if}
    <div class="panic-button" role="button" tabindex="0" on:click={() => window.open('https://clever.com', '_blank')} on:keydown={(e) => e.key === 'Enter' && window.open('https://clever.com', '_blank')}>!</div>
    <div class="panic-label">Panic</div>

    <div>
        <!-- Math problems removed -->
    </div>
</main>

<style>
    :global(body) {
        margin: 0;
        background-color: #050505;
        color: white;
        font-family: 'Inter', sans-serif;
        overflow-x: hidden;
        text-align: center;
    }

    .hero { text-align: center; padding: 50px 20px; }
    h1 { font-size: 3rem; letter-spacing: 5px; color: #00ff88; margin: 0; }

    .grid {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 20px;
        padding: 20px;
        max-width: 1000px;
        margin: 0 auto;
    }

    .card {
        background: #111;
        border: 2px solid #222;
        padding: 14px 18px;
        border-radius: 12px;
        cursor: pointer;
        transition: 0.2s;
        color: white;
        width: min(900px, 90%);
        height: 140px; /* slightly bigger */
        display: flex;
        align-items: center;
        gap: 16px;
        box-sizing: border-box;
        text-align: center;
    }

    .card-content { display:flex; flex-direction:column; justify-content:center; align-items:center; width:100%; }
    .card h2 { margin: 0 0 6px 0; font-size: 1.25rem; display:flex; align-items:center; gap:10px; }
    .card p { margin: 0; opacity: 0.95; font-size: 1rem; }

    .icon { font-size: 36px; width: auto; margin: 0; display:inline-flex; align-items:center; }

    .card:hover { border-color: #00ff88; transform: translateY(-5px); }

    /* PLAYER STYLES */
    .player-container {
        width: 100vw;
        height: 100vh;
        display: flex;
        flex-direction: column;
    }

    .controls {
        background: #111;
        padding: 10px 20px;
        display: flex;
        justify-content: space-between;
        align-items: center;
        border-bottom: 1px solid #333;
    }

    .game-title { font-weight: bold; color: #00ff88; }

    iframe {
        flex-grow: 1;
        border: none;
        background: black;
    }

    button {
        border: none;
        padding: 10px 18px;
        border-radius: 6px;
        font-weight: bold;
        cursor: pointer;
        transition: opacity 0.2s;
    }

    button:hover { opacity: 0.8; }

    .btn-primary { background: #00ff88; color: #000; }
    .btn-secondary { background: #333; color: white; }

    .panic-button {
        position: fixed;
        top: 10px;
        right: 10px;
        width: 40px;
        height: 40px;
        border-radius: 50%;
        background-color: black;
        color: white;
        display: flex;
        align-items: center;
        justify-content: center;
        font-size: 24px;
        cursor: pointer;
        transition: background-color 0.3s;
        border: 3px solid white; /* Added border for visibility */
    }

    .panic-button:hover {
        background-color: red;
    }

    .panic-label {
        position: fixed;
        top: 60px;
        right: 10px;
        width: 40px; /* match panic-button width */
        text-align: center;
        color: white;
        font-size: 16px;
    }
</style>