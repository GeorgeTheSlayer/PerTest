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

<button on:click={onClick}>Start</button>
<p>{x}</p>
<p>{y}</p>
<p>{z}</p>
