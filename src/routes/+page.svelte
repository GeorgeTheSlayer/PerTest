<script lang="ts">
    let x = 0;
    let y = 0;
    let z = 0;

    function onClick() {
        function handleMotionEvent(event: any) {
            x = event.accelerationIncludingGravity.x;
            y = event.accelerationIncludingGravity.y;
            z = event.accelerationIncludingGravity.z;

            // Do something awesome.
        }

        window.addEventListener("devicemotion", handleMotionEvent, true);

        addEventListener("deviceorientation", (event) => {});
    }
</script>

<h1>Welcome to SvelteKit</h1>
<p>
    Visit <a href="https://kit.svelte.dev">kit.svelte.dev</a> to read the documentation
</p>
<button on:click={onClick}>Start</button>
<p>{x}</p>
<p>{y}</p>
<p>{z}</p>
