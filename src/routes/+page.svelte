<script lang="ts">
//Helper Class
    interface accelerometer {
        x: number;
        y: number;
        z: number;
        lastY: number;
        lastX: number;
        lastZ: number;
    }

    let withoutGrav: accelerometer = {
        x: 0,
        y: 0,
        z: 0,
        lastY: 0,
        lastX: 0,
        lastZ: 0
    };

    let withGrav: accelerometer = {
        x: 0,
        y: 0,
        z: 0,
        lastY: 0,
        lastX: 0,
        lastZ: 0
    };

    let isRound: boolean = true;

    //Console Log For mobile Devices
    let text = 'Ready to Start';
    function setValues(e: DeviceMotionEvent, accel: accelerometer, grav: boolean = false) {
        if (grav) {
            accel.x = e.accelerationIncludingGravity.x;
            accel.y = e.accelerationIncludingGravity.y;
            accel.z = e.accelerationIncludingGravity.z;
        } else {
            accel.x = e.acceleration.x;
            accel.y = e.acceleration.y;
            accel.z = e.acceleration.z;
        }


        if (accel.lastY < accel.y) {
            accel.lastY = accel.y;
        }

        if (accel.lastX < accel.x) {
            accel.lastX = accel.x;
        }

        if (accel.lastZ < accel.z) {
            accel.lastZ = accel.z;
        }

        if (isRound){
            accel.x = Math.round(accel.x);
            accel.y = Math.round(accel.y);
            accel.z = Math.round(accel.z);
            accel.lastX = Math.round(accel.lastX);
            accel.lastY = Math.round(accel.lastY);
            accel.lastZ = Math.round(accel.lastZ);
        }


    }

    //Where the event is Handled
    function handleMotionEvent(event: DeviceMotionEvent) {
        //Helper Functions
        withGrav.x = event.accelerationIncludingGravity.x;
        withoutGrav.x = event.acceleration.x;

        setValues(event, withoutGrav, false);
        setValues(event, withGrav, true);

    }

    //Reset Values
    function reset() {
        withoutGrav.lastY = 0;
        withoutGrav.lastX = 0;
        withoutGrav.lastZ = 0;
        withGrav.lastY = 0;
        withGrav.lastX = 0;
        withGrav.lastZ = 0;
    }

    //On Click Init
    //Request Permission is needed for IOS Devices.
    //TODO: Test on Android
    function onClick() {
        DeviceOrientationEvent.requestPermission()
            .then(response => {
                if (response == 'granted') {
                    window.addEventListener("devicemotion", handleMotionEvent, true);
                    text = 'Started'
                }
            })
            .catch(console.error);
    }
</script>

<body style="text-align: center">
    <h1>Mobile Acceleration Test</h1>
    <h2>{text}</h2>
    <div style="display: flex; white-space: nowrap; overflow: hidden;">
        <div>
            <h1>With Gravity</h1>
            <p>X: {withoutGrav.x}</p>
            <p>Highest: {withoutGrav.lastX}</p>
            <p>Y: {withoutGrav.y}</p>
            <p>Highest: {withoutGrav.lastY}</p>
            <p>Z: {withoutGrav.z}</p>
            <p>Highest: {withoutGrav.lastZ}</p>
        </div>
        <div>
            <h1>Without Gravity</h1>
            <p>X: {withGrav.x}</p>
            <p>Highest: {withGrav.lastX}</p>
            <p>Y: {withGrav.y}</p>
            <p>Highest: {withGrav.lastY}</p>
            <p>Z: {withGrav.z}</p>
            <p>Highest: {withGrav.lastZ}</p>
        </div>
    </div>
    <div style="display: flex; flex: fit-content">
        <button on:click={onClick}>Start</button>
        <button on:click={reset}>Reset</button>
        <button on:click={() => isRound = !isRound}>Round</button>
    </div>
</body>