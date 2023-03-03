<script>
  import { onMount, tick } from 'svelte';
  import Sortable from "svelte-sortable"
  
  /*
  import EditorJS from '@editorjs/editorjs';
  import Header from '@editorjs/header'; 
  import Link from '@editorjs/link'; 
  import List from '@editorjs/list'; 
  import SimpleImage from '@editorjs/simple-image'; 
  import Embed from '@editorjs/embed'; 
  */
  
  let editor;
  let handle = ".handle"
  let darkmode = true;
  
  let items = [
    {
      "id": 1,
      "type": "title",
      "value": "Welcome to DocMatic.",
    },
    {
      "id": 2,
      "type": "subtitle",
      "value": "You can edit this text by simply clicking on it. To add a new title, paragraph, image or video, just click on the add button below."
    },
    {
      "id": 3,
      "type": "paragraph",
      "value": "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Sed lacus ipsum, suscipit at sem id, mattis viverra arcu. Etiam tempor dapibus enim vitae semper. Nullam auctor lorem viverra augue porta, ac fermentum nulla auctor. Pellentesque bibendum cursus eros, at dapibus felis. Cras consequat interdum velit vel auctor. Maecenas blandit est ut leo malesuada convallis. Sed cursus dapibus quam, at suscipit urna suscipit sit amet. Etiam sed faucibus eros, eu scelerisque neque. Maecenas sit amet nulla vel augue sodales dapibus ut a ipsum. Fusce mattis ultrices tincidunt. Proin et metus non dui volutpat convallis."
    }
  ];

  onMount(async () => {
    
    /*
    editor = new EditorJS({
      holder: 'editorjs',
      tools: {
        header: Header,
        link: Link,
        list: {
          class: List,
          inlineToolbar: true,
          config: {
            defaultStyle: "unordered",
          },
        },        
        image: SimpleImage,
        embed: Embed,
      },
      autofocus: true,
      readonly: true,
    });
    */
  init();
   
  });
  
  async function init(){
    await tick();
    document.querySelectorAll('.edit').forEach((el)=>{
      let id = el.id;
      new inLine('#'+id);
    })
  }
  
  function changeStyle(name){
    
    console.log(name)
    let style = `styles/${name}.css`;
    document.getElementById('style').setAttribute("href", style);
    
    let dark = ['Forma'];
    if(dark.includes(name)){
      darkmode = true;
    }else{
      darkmode = false;
    }
  }
  
  function addItem(type){
    let newItem = {}
    newItem.id = Date.now();
    newItem.type=type;
    newItem.value="";
    items.push(newItem);
    items = items;
    init()
  }
  
  function onChange() {
      //`items` are mutated
      console.log(items)
  }
</script>

<nav>
<div class="row">
  <div class="col-3">
    <img src="img/docmatic.png" id="logo" />
  </div>


  <div class="col-9 text-end">
    
 
    
    <span class="dropdown">
      <button class="btn btn-outline-dark dropdown-toggle" type="button" id="dropdownMenuButton1" data-bs-toggle="dropdown" aria-expanded="false">
        Style
      </button>
      <ul class="dropdown-menu" aria-labelledby="dropdownMenuButton1">
        <li><a class="dropdown-item" on:click={()=>changeStyle('Forma')}>Forma</a></li>
        <li><a class="dropdown-item" on:click={()=>changeStyle('Garomond')}>Garomond</a></li>
        
      </ul>
    </span>
    <button class="btn btn-outline-dark">Save</button>
  </div>
</div>
</nav>

<div class="wrap page" class:darkmode={darkmode} spellcheck="false">
  

  <Sortable {items}
  let:item={item}
  on:change={onChange} bind:handle={handle}>
  
  <div class="item">
    <div class="row">
      <div class="col-1">
        <i class="fa-solid fa-grip-vertical handle"></i>
      </div>
      <div class="col-11">
        <div class="edit {item.type}" id="item-{item.id}">{@html item.value}</div>
      </div>
    </div>
  </div>
  </Sortable>

  <div class="row">
    <div class="col-1 no-brdr">
      
    </div>
    <div class="col-11">
      
      <span class="dropdown mt-3">
          <button class="btn btn-outline-dark dropdown-toggle" type="button" id="dropdownMenuButton1" data-bs-toggle="dropdown" aria-expanded="false">
            Add
          </button>
          <ul class="dropdown-menu" aria-labelledby="dropdownMenuButton1">
            <li><a class="dropdown-item" on:click={()=>addItem('title')}>Title</a></li>
            <li><a class="dropdown-item" on:click={()=>addItem('subtitle')}>Subtitle</a></li>
            <li><a class="dropdown-item" on:click={()=>addItem('paragraph')}>Paragraph</a></li>
          </ul>
      </span>
      
      
    </div>
  </div>
  



</div>


<style>
 .wrap{
    margin: 0 auto;
    max-width: 768px;
    min-height: 800px;
    padding: 40px;
    border: 1px solid black;
    margin-top: 40px;
    margin-bottom: 40px;
  }
  
  nav{
    padding: 10px 15px 10px 15px;
    border-bottom: 2px solid black;
    background-color: white;
  }
  
  .col-1{
   border-right: 5px solid black; 
  }
  
  .no-brdr{
    border: 0 !important;
  }
  
  .page .col-11{
   padding-left: 50px;
  }
  
  .item{
    margin-bottom: 1em;
  }
  
  .edit{
  
    display: block !important;
    min-height: 20px;
  }
  
  .handle{

    cursor: grab;
  }
  
  .paragraph{
    margin-bottom: 1em;
  }
  
  #logo{
    width: 60px;
    margin-top: 2px;
  }
  
  nav h1{
      color: #000;
      font-family: adobe-garamond-pro, Adobe Garamond Pro, garamond, Times,
        serif;
      font-size: 30px;
      line-height: 44px;
      letter-spacing: 0.11em;
      text-transform: uppercase;
      font-weight: 400;
      font-style: normal;
      margin-bottom: 0;
  }
  
  .btn-outline-dark{
    border: 2px solid black;
    font-weight: 600;
  }
  
  .margin{
    height: 0px;
  }
  
  .darkmode  .col-1{
   border-right: 5px solid white; 
  }
  
  .darkmode .handle{
    color: white;
  }
  
  .darkmode .btn-outline-dark{
    color: white;
    border: 2px solid white;
  }
  
 
  
  .darkmode .title, .darkmode .subtitle, .darkmode .paragraph{
    color: white;
  }
  
  @media only screen and (max-width: 768px) {
    .wrap{
      width: 100%;
      padding: 15px;
      margin-top: 0;
      margin-bottom: 0;
    }
    
    .mob-hide{
      display: none;
    }
  }
 
</style>

