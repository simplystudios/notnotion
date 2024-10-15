<script lang="js">
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

function bold() {
    document.execCommand('bold', false, null);
    const textarea = document.getElementById("fake_textarea");
    textareavalue = textarea.innerHTML;

    // Move the cursor to the end of the selection and toggle bold off
    const selection = window.getSelection();
    if (selection.rangeCount > 0) {
        const range = selection.getRangeAt(0);
        range.collapse(false); // Collapse the range to the end point
        selection.removeAllRanges();
        selection.addRange(range);
        document.execCommand('bold', false, null); // Toggle bold off
    }
}

function addInput() {
    console.log("addInput function called"); // Add this line
    const input = document.createElement("input");
    input.type = "text";
    input.placeholder = "New Input";
    input.style.fontSize = "2em"; // Set the font size to match the h1 element

    const container = document.getElementById("input_container");
    if (container) {
        container.appendChild(input); // Appends the new input to the container
    } else {
        console.error("Element with id 'input_container' not found.");
    }
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
            <FormatHeader1 on:click={addInput} {size} {color}/>
        </button>
        <button class="but2">
            <FormatHeader2 {size} {color}/>
        </button>
        <button class="but2" on:click={() => bold()}>
            <FormatBold {size} {color}/>
        </button>
    </div>
    <div id="input_container"></div>
    <div id="texta" class="reversed">
        <div class="tarea" id="fake_textarea" contenteditable style={`color: ${color};`} placeholder="Type something..." bind:innerHTML={textareavalue}></div>
    </div>
</div>

<style>
    :root {
        --color: #191919;
    }
    .area {
        transition: all .2s ease-in-out;
        font-family: 'Roboto';
        margin: 10px;
    }
    .tarea {
        width: 100%;
        overflow:visible;
        height: 500px;
        border: none;
        background-color: transparent;
        margin: 12px;
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
        color: rgb(169, 162, 162)
    }
    .but::after{
        color: grey;
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
