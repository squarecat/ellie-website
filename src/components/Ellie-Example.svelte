<script>
  import { onMount } from "svelte";

  export let input = "";
  export let output = "";
  export let name;
  export let subject;
  export let arrow;

  let words = output.match(/<[^>]+>|[^<\s]+|\s+/g) || [];
  let displayText = "";
  let index = 0;

  function startTyping() {
    const interval = setInterval(() => {
      if (index < words.length) {
        displayText += words[index++];
        document.getElementById(`${name}-output`).innerHTML = displayText;
      } else {
        clearInterval(interval);
      }
    }, 15);
  }
  onMount(() => {
    document.getElementById(`${name}-input`).innerHTML = input;
    document.getElementById(`${name}-placeholder`).innerHTML = output;
    const boxHeight = document
      .getElementById(`${name}-input`)
      .getBoundingClientRect().height;

    document.getElementById(`ellie-image-${name}`).style.top = `${
      boxHeight + 60
    }px`;
    document.getElementById(`ellie-reply-box-${name}`).style.top = `${
      boxHeight + 160
    }px`;
    setTimeout(startTyping, 1200);
  });
</script>

<div class="ellie-example">
  <div class="box">
    <div class="subject">
      <span class="label">Subject</span>
      <span class="value">{subject}</span>
    </div>
    <div id={`${name}-input`} class="typed-text"></div>
  </div>

  <div class="ellie-image-container" id="ellie-image-{name}">
    <picture role="presentation" class="ellie-reply-image">
      <!-- <source
        media="(max-width: 850px)"
        srcset="/assets/images/arrows/{arrow}.png"
      /> -->
      <img src="/assets/images/arrows/{arrow}.png" alt="" />
    </picture>
  </div>

  <div class="box ellie-reply" id="ellie-reply-box-{name}">
    <div class="subject">
      <span class="label">Subject</span>
      <span class="value">RE: {subject}</span>
    </div>
    <div class="ellie-reply-output">
      <div id={`${name}-output`} class="typed-text visible"></div>
      <div id={`${name}-placeholder`} class="typed-text invisible"></div>
    </div>
  </div>
</div>

<style>
  .ellie-example {
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    width: 100%;
    width: 95vw;
    max-width: 630px;
    min-height: 420px;
    z-index: 2;
    padding: 0 15px;
  }

  @media (max-width: 850px) {
    .ellie-example {
      flex-direction: column;
    }
  }

  @media (max-width: 550px) {
    .ellie-example {
      min-height: 600px;
      margin-bottom: 100px;
    }
  }

  .box {
    position: absolute;
    top: 0;
    left: 0;
    overflow: hidden;
    border-radius: 10px;
    border: 1px solid #e1e3e1;
    background-color: #fefefe;
    width: 450px;
    font-family: Arial, Helvetica, sans-serif;
    font-size: 13px;
    padding-bottom: 8px;
    box-shadow:
      -1px -1px 2px rgba(0, 0, 0, 0.1),
      -4px -4px 5px rgba(255, 255, 255, 1),
      2px 2px 5px rgba(0, 0, 0, 0.1);
  }

  @media (max-width: 700px) {
    .box {
      width: 350px;
    }
  }

  @media (max-width: 550px) {
    .box {
      width: 100%;
    }
  }

  .box.ellie-reply {
    left: 100%;
    top: 90px;
    transform: translate(-100%, -200px);
  }

  @media (max-width: 550px) {
    .box.ellie-reply {
      top: 290px;
      left: auto;
      transform: none;
    }
  }

  .subject {
    margin-bottom: 10px;
    border-bottom: 1px solid #e1e3e1;
  }
  .label {
    display: inline-block;
    padding: 6px 6px;
    background-color: rgb(242, 236, 255);
  }
  .value {
    display: inline-block;
    padding: 6px 0 6px 6px;
  }
  .ellie-reply-output {
    position: relative;
  }
  .ellie-image-container {
    position: absolute;
    width: 130px;
    left: 40px;
    top: 245px;
    margin: -1px 0 -5px 0;
    z-index: 2;
  }
  .ellie-reply-image {
    width: 120px;
    margin: -4px 0 -2px 0;
    min-width: 220px;
  }
  @media (max-width: 850px) {
    .ellie-reply-image {
      min-width: 160px;
      margin: -4px 0 -2px 0;
    }
  }

  .typed-text {
    padding: 4px 8px;
    font-size: 13px;
    font-family: Arial, Helvetica, sans-serif;
    white-space: pre-wrap;
    white-space: -moz-pre-wrap;
    white-space: -pre-wrap;
    white-space: -o-pre-wrap;
    word-wrap: break-word;
  }

  .typed-text.visible {
    position: absolute;
    top: 0;
  }
  .typed-text.invisible {
    visibility: hidden;
  }
</style>
