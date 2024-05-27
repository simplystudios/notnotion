<script>
// @ts-nocheck
import ThemeLightDark  from "svelte-material-icons/ThemeLightDark.svelte";
import FormatHeader1  from "svelte-material-icons/FormatHeader1.svelte";
import FormatHeader2  from "svelte-material-icons/FormatHeader2.svelte";
import FormatBold  from "svelte-material-icons/FormatBold.svelte";
import ContentSave  from "svelte-material-icons/ContentSave.svelte";
import { ModeWatcher, setMode, mode } from "mode-watcher";

let windowtitle = '';
let color = "gray";
let size = "20";
let textareavalue;
function saveastxt() {
    const textareavalue = document.querySelector(".tarea").value; // Get the value from the textarea
    const blob = new Blob([textareavalue], { type: "text/plain" });
    const fileurl = URL.createObjectURL(blob);
    const link = document.createElement("a");
    link.download = windowtitle.value || 'untitled.txt'; // Provide a default filename if windowtitle is empty
    link.href = fileurl;
    link.click();
}

function bold(){
    
    document.execCommand(bold,false,null)
}

function addh1() {
    const h1 = document.createElement("h1");
    h1.textContent = "New Header";
    const element = document.getElementById("texta");
    element.insertBefore(h1, element.firstChild); // Inserts the new header at the beginning of the texta div, respecting the reversed order
}

function toggletheme() {
    if ($mode === "light") {
        setMode("dark");
        document.querySelector('body').style = "background-color:#191919;";
        color = "gray";
    } else {
        setMode("light");
        document.querySelector('body').style = "background-color:white;";
        color = "gray";
    }
}

function addnewtask() {
    window.open("/", '_blank');
    document.querySelector('title').textContent = windowtitle.value;
}

</script>

<ModeWatcher />
<slot/>

<div class="area">
    <div class="controls">
        <button class="but" on:click={addnewtask}>
            <span>+</span>
        </button>
        <input class="inp" style={`color: ${color};`} type="Title" bind:this={windowtitle} name="Title..." placeholder="Title" id="">
        <button class="but" on:click={toggletheme}>
            <ThemeLightDark {color} {size} />
        </button>
        <button class="but2" on:click={() => saveastxt()}>
            <ContentSave {size} {color}/>
        </button>
    </div>
    <div class="controls2">
        <button class="but2">
            <FormatHeader1 on:click={addh1} {size} {color}/>
        </button>
        <button class="but2">
            <FormatHeader2 {size} {color}/>
        </button>
        <button class="but2" on:click={() => bold()}>
            <FormatBold {size} {color}/>
        </button>
    </div>
    <div id="texta" class="reversed">
        <textarea bind:this={textareavalue} style={`color: ${color};`} class="tarea" placeholder="Type something..." id=""></textarea>
    </div>
</div>

<style>
    :root {
        --color: #191919;
    }
    .area {
        margin: 10px;
    }
    .tarea {
        width: 100%;
        height: 500px;
        border: none;
        background-color: transparent;
        margin: 10px;
        font-size: 15px;
    }
    .tarea:focus {
        outline: none;
    }
    .controls {
        display: flex;
    }
    .controls2 {
        display: flex;
        margin: 10px;
        border: 1px;
        border-radius: 10px;
        border-color: gray;
        border-style:double;
        padding: 10px;
    }
    .but {
        background-color: transparent;
        border: 0;
        height: 50px;
        font-size: 30px;
        margin: 10px;
        color: grey;
    }
    .but2 {
        background-color: transparent;
        border: 0;
        color: grey;
    }
    .but:hover {
        color: var(--color);
    }
    .inp {
        width: 100%;
        border: none;
        background-color: transparent;
        font-size: 20px;
    }
    .inp:focus {
        border: none;
        outline: none;
    }
    .reversed {
        display: flex;
        flex-direction: column-reverse; /* This reverses the vertical order */
    }
</style>
