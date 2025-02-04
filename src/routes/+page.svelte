<script lang="js">
// @ts-nocheck
 import { SunMoon, Heading1, Heading2, Bold, Italic, Code, Text, Save, GripVertical } from "lucide-svelte";
import { ModeWatcher, setMode, mode } from "mode-watcher";
import "@friendofsvelte/tipex/styles/Tipex.css";
import {Utility} from "@friendofsvelte/tipex";
import "@friendofsvelte/tipex/styles/ProseMirror.css";
import "@friendofsvelte/tipex/styles/Controls.css";
import "@friendofsvelte/tipex/styles/EditLink.css";
import "@friendofsvelte/tipex/styles/CodeBlock.css";
import {Tipex} from '@friendofsvelte/tipex';
 import { onMount } from "svelte";



let tipexval = '';
let body = ``;
let windowtitle = '';
let colorb = '';
let color = "#cfd0d4";
let size = "20";
let textareavalue;
let editor;
function saveastxt() {
        const textareavalue = editor.getHTML(); // Get the value from the textarea
        console.log(textareavalue)
        const blob = new Blob([textareavalue], { type: "text/plain" });
        const fileurl = URL.createObjectURL(blob);
        const link = document.createElement("a");
        link.download = windowtitle.value || 'untitled.txt'; // Provide a default filename if windowtitle is empty
        link.href = fileurl;
        link.click();
    }
    

     onMount(() => {
      const prefersDark = window.matchMedia("(prefers-color-scheme: dark)").matches;
      if (prefersDark) {
          setMode("dark");
          document.documentElement.style.setProperty('--border-color', 'rgb(32, 31, 31)');
          document.body.style.backgroundColor = "#191919";
          color = "#cfd0d4";
      } else {
          setMode("light");
          document.documentElement.style.setProperty('--border-color', '#edeceb');
          document.body.style.backgroundColor = "white";
          color = "gray";
      }
  });


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
        color = "#cfd0d4";
        document.querySelector('body').style = "background-color:#191919;";
        document.documentElement.style.setProperty('--border-color', 'rgb(32, 31, 31)');
    } else {
        setMode("light");
        document.querySelector('body').style = "background-color:white;";
        color = "gray";
        document.documentElement.style.setProperty('--border-color', '#edeceb');

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
        <input class="inp" style={`color: ${color};`} type="Title" bind:value={windowtitle} name="Title..." placeholder="Title" id="">
        <button class="but" on:click={toggletheme}>
            <SunMoon  {color} {size} />
        </button>
        <button class="but" on:click={() => saveastxt()}>
            <Save  {size} {color}/>
        </button>
    </div> 
    <hr class="area">
    <div class="controls2">
        <button on:click={() => editor.chain().focus().toggleHeading({ level: 1 }).run()} class:active={editor?.isActive('heading', { level: 1 })} class="but2" aria-label="Heading 1" type="button"><Heading1  {size} {color}/></button>
        <button on:click={() => editor.chain().focus().toggleHeading({ level: 2 }).run()} class:active={editor?.isActive('heading', { level: 2 })} class="but2" aria-label="Heading 2" type="button"><Heading2  {size} {color}/></button>
        <button on:click={() => editor.chain().focus().setParagraph().run()} class:active={editor?.isActive('paragraph')} class="but2" aria-label="Italic" type="button"><Text  {size} {color}/></button>
        <button on:click={() => editor.chain().focus().toggleBold().run()} class:active={editor?.isActive('bold')} class="but2" aria-label="Bold" type="button"><Bold  {size} {color}/></button>
        <button on:click={() => editor.chain().focus().toggleItalic().run()} class:active={editor?.isActive('italic')} class="but2" aria-label="Italic" type="button"><Italic {size} {color}/></button>
        <button on:click={() => editor.chain().focus().toggleCodeBlock().run()} class:active={editor?.isActive('codeBlock')} class="but2" aria-label="Code block" type="button"><Code  {size} {color}/></button>    

    </div>
    

    <!-- <div id="input_container"></div>
     <div id="texta" class="reversed">
        <div class="tarea" id="fake_textarea" contenteditable style={`color: ${color};`} placeholder="Type something..." bind:innerHTML={textareavalue}></div>
    </div> -->
    <!-- <div class="controls">
            <button on:click={() => editor.chain().focus().toggleHeading({ level: 1 }).run()} class:active={editor?.isActive('heading', { level: 1 })} class="tipex-edit-button tipex-button-extra tipex-button-rigid" aria-label="Heading 1" type="button">H1</button>
            <button on:click={() => editor.chain().focus().toggleHeading({ level: 2 }).run()} class:active={editor?.isActive('heading', { level: 2 })} class="tipex-edit-button tipex-button-extra tipex-button-rigid" aria-label="Heading 2" type="button">H2</button>
            <button on:click={() => editor.chain().focus().setParagraph().run()} class:active={editor?.isActive('paragraph')} class="tipex-edit-button tipex-button-extra tipex-button-rigid" aria-label="Paragraph/Normal text" type="button">P</button>
            <button on:click={() => editor.chain().focus().toggleBold().run()} class:active={editor?.isActive('bold')} class="tipex-edit-button tipex-button-extra tipex-button-rigid" aria-label="Bold" type="button">B</button>
            <button on:click={() => editor.chain().focus().toggleItalic().run()} class:active={editor?.isActive('italic')} class="tipex-edit-button tipex-button-extra tipex-button-rigid" aria-label="Italic" type="button">I</button>
            <button on:click={() => editor.chain().focus().toggleUnderline().run()} class:active={editor?.isActive('underline')} class="tipex-edit
            -button tipex-button-extra tipex-button-rigid" aria-label="Underline" type="button">U</button>
            <button on:click={() => editor.chain().focus().toggleStrike().run()} class:active={editor?.isActive('strike')} class="tipex-edit-button tipex-button-extra tipex-button-rigid" aria-label="Strikethrough" type="button">S</button>  
            <button on:click={() => editor.chain().focus().toggleCodeBlock().run()} class:active={editor?.isActive('codeBlock')} class="tipex-edit-button tipex-button-extra tipex-button-rigid" aria-label="Code block" type="button">Code</button>    
        </div> -->
        
        <Tipex {body} bind:tipex={editor} focused
    style=" width:100%; margin-bottom: 0;" 
    class="h-[100%] border border-neutral-500">

</Tipex>


</div>

<style>
    :root {
        --color: #161716;
        --border-color : rgb(32, 31, 31);
    }
    .area {
        transition: all .2s ease-in-out;
        font-family: 'Roboto';
        border: 1;
        border-radius: 10px;
        border-style: solid;
        border-color: var(--border-color);
        margin: 10px;
    }
    .tarea {
        transition: all .2s ease-in-out;
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
       
        
        padding: 10px;
        padding: 10px;
    }
    .but {
        background-color: transparent;
        border: 0;
        height: 50px;
        font-size: 30px;
        margin: 8px;
        color: grey;
    }
    .but2 {
        background-color: transparent;
        border: 0;
        color: grey;
        padding: 5px;
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
