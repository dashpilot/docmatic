<script>
  import { onMount, tick } from 'svelte';
  import Sortable from "svelte-sortable"

  import { account } from './lib/appwrite';
  
  
  import Appwrite from "./lib/Appwrite.svelte"
  let showModal = false;
  
  let editor;
  let handle = ".handle"
  let darkmode = true;
  
  let mode = 'signup';
  let user = false;
  
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
  
    init();

 
    user = await account.get();
    console.log(user)

  
  /*
    userbase.init({ appId: '896a0bf8-bab5-4325-93f0-566423bf201c' })
    .then((session) => {
      console.log(session.user)
      user = session.user;
    })
    .catch(() => user = false)
    */
   
  });
  
  async function logout() {
        await account.deleteSession('current');
        user = false;
    }
  
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
    newItem.value2="";
    newItem.value3="";
    items.push(newItem);
    items = items;
    init()
  }

  function removeItem(id){
    if(confirm("Are you sure you wish to delete this item?")){
      items = items.filter(item => item.id !== id);
    }
  }
  
  function onChange() {
      //`items` are mutated
      console.log(items)
  }
  
  /*
  function account(mymode){
    mode = mymode;
    showModal=true;
  }
    */
  
  function save(){
    alert('coming soon')
  }
</script>

<nav>
<div class="row">
  <div class="col-5">
    <span class="dropdown">
      <button class="btn btn-outline-dark dropdown-toggle" type="button" id="dropdownMenuButton1" data-bs-toggle="dropdown" aria-expanded="false">
        Style
      </button>
      <ul class="dropdown-menu" aria-labelledby="dropdownMenuButton1">
        <li><a class="dropdown-item" on:click={()=>changeStyle('Forma')}>Forma</a></li>
        <li><a class="dropdown-item" on:click={()=>changeStyle('Playfair')}>Playfair</a></li>
        <li><a class="dropdown-item" on:click={()=>changeStyle('Garomond')}>Garomond</a></li>
       
        
      </ul>
    </span>
  </div>
<div class="col-2 text-center">
<img src="img/docmatic.png" id="logo" />
</div>

  <div class="col-5 text-end">
    

  
  
 
 

    {#if !user}
  
    <button class="btn btn-outline-dark" on:click={()=>showModal=true}>Sign In</button>

    <Appwrite bind:showModal />

    {:else}
    <button class="btn btn-outline-dark" on:click={save}>Save</button>
    <button class="btn btn-outline-dark" on:click={logout}><i class="fas fa-sign-out"></i></button>
    {/if}




  </div>
</div>
</nav>

<div class="wrap page" class:darkmode={darkmode} spellcheck="false">
  

  <Sortable {items}
  let:item={item}
  on:change={onChange}>
  
  <div class="item">
    <div class="row">
      <div class="col-1">
        <i class="fa-solid fa-grip-vertical handle"></i>
        <div></div>
        <i class="fa-solid fa-trash" on:click={()=> removeItem(item.id)}></i>
      </div>
      <div class="col-11">

{#if item.type=='title' || item.type=='subtitle' || item.type=='paragraph'}
<div class="edit {item.type}" id="item-{item.id}">{@html item.value}</div>
{/if}

{#if item.type=='two-col'}
<div class="row">
  <div class="col-md-6 edit {item.type}" id="item-{item.id}">{@html item.value}</div>
  <div class="col-md-6 edit {item.type}" id="item-{item.id}-2">{@html item.value2}</div>
</div>
{/if}

       
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
            <li><a class="dropdown-item" on:click={()=>addItem('two-col')}>Two Columns</a></li>
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
      padding: 40px 20px;
      margin-top: 0;
      margin-bottom: 0;
    }
    
    .mob-hide{
      display: none;
    }
  }
 
</style>

