<script>
  import { marked } from 'marked'
  let markdown = '# Marked in Node.js\n\nRendered by **marked**.'
  $: html = marked.parse(markdown);

</script>

<div class="flex">
  <!-- div editable -->
  <div class="container" id="editable--container" contenteditable="true" bind:innerText={markdown} />
  <div class="container" id="preview--container">
    preview {@html html}
  </div>
  <div class="container" id="raw--container">
    {html}
  </div>
  <div class="container" id="style--container">
    style
  </div>
</div>

<style>
  .flex{
    display: flex;
  } 

  .flex .container{
    flex: 1;
    border: 1px solid #ccc;
    padding: 10px;
  }
</style>