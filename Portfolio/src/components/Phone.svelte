<script>
    // Imports
    import DeviceContent from "./DeviceContent.svelte";

    // Vars
    export let img = "";
    export let src = "";
    export let gif = "";
    export let hasDesktopPreview = true;
    export let hasMobilePreview = true;

    // Fullscreen Button
    let fullscreenPreview = false;

    document.addEventListener("fullscreenchange", () => {
        // console.log("fullscreenchange");
        fullscreenPreview = !fullscreenPreview;
    });

    if (document.addEventListener) {
        document.addEventListener("webkitfullscreenchange", exitHandler, false);
        document.addEventListener("mozfullscreenchange", exitHandler, false);
        document.addEventListener("fullscreenchange", exitHandler, false);
        document.addEventListener("MSFullscreenChange", exitHandler, false);
    }

    function exitHandler() {
        fullscreenPreview = !fullscreenPreview;
    }

    function toggleFullScreen() {
        var game = document.getElementsByClassName("game")[0];
        if (fullscreenPreview) {
            closeFullscreen();
        } else {
            requestFullScreen(game);
        }
    }

    function requestFullScreen(element) {
        let requestMethod =
            element.requestFullScreen ||
            element.webkitRequestFullScreen ||
            element.mozRequestFullScreen ||
            element.msRequestFullscreen;

        if (requestMethod) {
            // Native full screen.
            requestMethod.call(element);
        } else if (typeof window.ActiveXObject !== "undefined") {
            // IE Backup
            var wscript = new ActiveXObject("WScript.Shell");
            if (wscript !== null) {
                wscript.SendKeys("{F11}");
            }
        }
    }

    function closeFullscreen() {
        if (document.exitFullscreen) {
            document.exitFullscreen();
        } else if (document.webkitExitFullscreen) {
            /* Safari */
            document.webkitExitFullscreen();
        } else if (document.msExitFullscreen) {
            /* IE11 */
            document.msExitFullscreen();
        }
    }
</script>

<div class="phone">
    <div class="phone__top">
        <div class="phone__speaker" />
    </div>
    <div class="phone__mid">
        <div class="game" class:fullscreen={fullscreenPreview === true}>
            <DeviceContent
                {img}
                {src}
                {gif}
                {hasDesktopPreview}
                {hasMobilePreview}
            />

            <!-- <img
                src="/assets/icons/fullscreen2.png"
                class="fullscreenToggle"
                alt="toggle fullscreen button"
                on:click={toggleFullScreen}
            /> -->
        </div>
    </div>
    <div class="phone__bot">
        <div class="phone__home" />
    </div>
</div>

<style>
    .phone {
        /* height: 960px; */
        max-height: 90%;
        aspect-ratio: 9/16;
        background-color: rgb(1, 16, 11);
        border-radius: 10% / 5%;
        display: flex;
        flex-direction: column;
        box-shadow: 3px 5px 12px rgb(82, 82, 82);
    }

    .phone__top {
        flex: 1;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .phone__speaker {
        background-color: rgb(41, 41, 41);
        width: 50%;
        height: 10%;
        border-radius: 5px;
    }

    .phone__mid {
        width: 90%;
        /* height: 90%; */
        flex: 9;
        margin: auto;
    }

    .phone__bot {
        flex: 1;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .phone__home {
        height: 55%;
        aspect-ratio: 1/1;
        border: rgb(41, 41, 41) 3px solid;
        border-radius: 50px;
    }

    .game {
        width: 100%;
        height: 100%;
        position: relative;
        border-radius: 10% / 5%;
        overflow: hidden;
    }

    .fullscreen {
        width: 100%;
        height: 100%;
        position: absolute;
        top: 0;
        left: 0;
    }

    .fullscreen iframe {
        border-radius: 0px;
    }

    .fullscreen .fullscreenToggle {
        width: 3%;
    }

    .fullscreenToggle {
        border-radius: 5px 0px 15px 0px;
        background-color: rgba(255, 255, 255, 0);
        position: absolute;
        right: 0;
        bottom: 0;
        width: 10%;
        padding: 3px;
        -webkit-transition: background-color 250ms linear;
        -ms-transition: background-color 250ms linear;
        transition: background-color 250ms linear;
        cursor: pointer;
    }

    .fullscreenToggle:hover {
        background-color: rgba(255, 255, 255, 0.304);
        -webkit-transition: background-color 250ms linear;
        -ms-transition: background-color 250ms linear;
        transition: background-color 250ms linear;
    }
</style>
