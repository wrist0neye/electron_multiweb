<script>
  import { onMount } from "svelte";

  let itemDiv; // tag만 가져와서 쓸모 없음
  let classState = "idle";
  let beforeState = ""
  let lastdirection = "center";
  let lasttarget = "";
  

  let dimensions = {};

  onMount(()=> {
    let container = document.getElementsByClassName("container");
    console.log(container)
    // container.onresize = (e) => {console.log('resize')}
    // let observer = new ResizeObserver(resizeEvent)
    // observer.observe(container)
    
    dimensions.width = container[0].clientWidth;
    dimensions.height = container[0].clientHeight;
    dimensions.offsetX = container[0].offsetLeft;
    dimensions.offsetY = container[0].offsetTop;
    dimensions.upsideRegion = dimensions.height * 0.1 + dimensions.offsetY;
    dimensions.downsideRegion = dimensions.height * 0.9 + dimensions.offsetY;
    dimensions.leftsideRegion = dimensions.width * 0.2 + dimensions.offsetX;
    dimensions.rightsideRegion = dimensions.width * 0.8 + dimensions.offsetX;

    console.log(dimensions);
  })

  const dropEvent = (e) => {
    console.log(e)
    console.log(`dragEnd ==> ${lasttarget} : ${lastdirection}`)
  }

  const dragover = (event) => {
    console.log(`(xpos, ypos) = (${event.layerX},${event.layerY})`);
    console.log(`(xsize, ysize) = (${itemDiv.width}, ${itemDiv.height})`);
  }

  const highlighting = (e) => {
    let container = document.getElementsByClassName("container");
    // let item = document.getElementsByClassName("box_preview");
    // container.onresize = (e) => {console.log('resize')}
    // let observer = new ResizeObserver(resizeEvent)
    // observer.observe(container)

    dimensions.width = container[0].clientWidth;
    dimensions.height = container[0].clientHeight;
    dimensions.offsetX = container[0].offsetLeft;
    dimensions.offsetY = container[0].offsetTop;
    dimensions.upsideRegion = dimensions.height * 0.1 + dimensions.offsetY;
    dimensions.downsideRegion = dimensions.height * 0.9 + dimensions.offsetY;
    dimensions.leftsideRegion = dimensions.width * 0.2 + dimensions.offsetX;
    dimensions.rightsideRegion = dimensions.width * 0.8 + dimensions.offsetX;
    
    console.log(e)
    // 보통 모니터는 가로로 기니까 가로 hover기능 우선도를 높인다.
    if(e.clientX < dimensions.leftsideRegion) {
      // left attach
      console.log(`Left align : ${e.clientX} < ${dimensions.leftsideRegion}`);
      classState = "left_align";
      beforeState = "left_attach";
      lastdirection = "left";
    } else if(e.clientX > dimensions.rightsideRegion) {
      // right attach
      console.log(`Right align : ${e.clientX} > ${dimensions.rightsideRegion}`);
      classState = "right_align";
      beforeState = "right_attach";
      lastdirection = "right";
    } else if(e.clientY < dimensions.upsideRegion) {
      // top attach
      console.log(`Top align : ${e.clientY} < ${dimensions.upsideRegion}`);
      classState = "up_align";
      beforeState = "up_attach";
      lastdirection = "up";
    } else if(e.clientY > dimensions.downsideRegion) {
      // bottom attach
      console.log(`Bottom align : ${e.clientY} > ${dimensions.downsideRegion}`);
      classState = "down_align";
      beforeState = "down_attach"
      lastdirection = "down";
    } else {
      // idle state
      classState = 'idle_align';
    }
    
    // lasttarget = document.querySelector('#preview_1') // 위와 같은 구문
    // lasttarget = document.getElementById('preview_1') // 위와 같은 구문
    
    lasttarget = e.target
    console.log(lasttarget)
  }

  const dragleavefunc = (e) => {
    classState = "idle"
    lasttarget = ""
    lastdirection = "center"
    // console.log(`${lasttarget} : ${lastdirection}`)
  }

  const dropEventfunc = (e) => {
    classState = "idle"
    console.log(`dropEventfunc ==> ${lasttarget} : ${lastdirection}`)
  }

</script>

<style>
  .container{
    position: relative;

    width: 100%;
    height: 80%;
    border: 2px dashed black;
    border-radius: 1rem;
    background-color: grey;
    margin-bottom: 10px;

    display: flex;
    flex-direction:row;
    justify-content: space-around;
    align-items: center;
  }
  
  .box_preview{
    position: absolute;
    border-radius: 1rem;
    /* pointer-events: none; */
    
    transition: all 0.1s ease;
  }

  .idle {
    width:100%;
    height:100%;
    background-color: rgba(255,255,0,0);
  }

  .left_attach{
    left:0%;
  }
  .right_attach{
    right:0%;
  }
  .up_attach{
    top:0%;
  }
  .down_attach{
    bottom:0%;
  }

  .left_align{
    width:50%;
    height:100%;
    left:0%;
    background-color: rgba(255,255,255,0.4);
  }

  .right_align{
    width:50%;
    height:100%;
    right:0%;
    background-color: rgba(255,255,255,0.4);
  }

  .up_align{
    width:100%;
    height:50%;
    top:0%;
    background-color: rgba(255,255,255,0.4);
  }

  .down_align {
    width:100%;
    height:50%;
    bottom:0%;
    background-color: rgba(255,255,255,0.4);
  }

  .idle_align {
    width:100%;
    height:100%;
    background-color: rgba(255,255,255,0.4);
  }

  .item{
    width: 40px;
    height: 40px;
    border-radius: 5px;
    background-color: blueviolet;
    box-shadow: 4px 2px 2px white;
    cursor:grab;
  }
</style>

<div 
  class="item"
  draggable="true"
  on:click = {e=>console.log('item is clicked')}
  on:dragstart = {e=>console.log('drag start')}
  on:dragend = {e=>{dropEvent(e);}}></div>
<div
  class="container"
  id="browser1"
  on:dragover|stopPropagation = {e=>{highlighting(e)}}
  on:dragleave|stopPropagation = {e=>{dragleavefunc(e)}}
  on:drop|stopPropagation = {e=>dropEventfunc(e)}>
  <!-- <p>
    Lorem ipsum dolor sit amet, consectetur adipiscing elit. Aenean non lacus odio. Orci varius natoque penatibus et magnis dis parturient montes, nascetur ridiculus mus. Suspendisse risus augue, commodo sit amet pulvinar vel, varius vel felis. Nam porta egestas tempus. Pellentesque in ullamcorper quam. Cras at elit vel mauris lacinia consectetur. Proin ornare lobortis ipsum, a aliquam magna congue hendrerit.
    Class aptent taciti sociosqu ad litora torquent per conubia nostra, per inceptos himenaeos. Nulla pulvinar luctus neque. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Curabitur in nisl felis. Nam gravida ipsum at libero posuere laoreet. Sed at molestie turpis, quis dictum arcu. Sed quis erat varius, lobortis tellus in, hendrerit ex. Vivamus fringilla nisl a elit volutpat, sed iaculis odio mollis. Praesent sed euismod ligula. Aliquam venenatis aliquam ultricies. Cras aliquet dictum lorem, eget semper sem malesuada ac.
    In suscipit dapibus quam vel porta. Aliquam erat volutpat. In lacinia massa tellus. Nunc eu arcu id ex tristique aliquam ac sed nulla. Quisque rutrum cursus nisl, quis interdum nisl finibus et. Duis fermentum erat vehicula suscipit volutpat. Morbi finibus enim at risus venenatis, eget sollicitudin felis malesuada. Phasellus molestie ipsum metus, sit amet congue mauris feugiat ut. Curabitur molestie viverra nisl in pharetra. Morbi nec lacinia ante, varius molestie tellus. Nullam et justo nec sem mattis rhoncus sed ut enim.
    Pellentesque rhoncus, tellus sed viverra laoreet, ante mauris auctor quam, quis venenatis justo eros ut magna. Pellentesque tellus sem, pellentesque nec tristique eget, faucibus nec lacus. Sed nec dapibus eros. Quisque feugiat mattis vestibulum. Fusce iaculis iaculis risus. Aliquam a faucibus sem. Praesent quis dapibus diam, sit amet elementum odio. Nunc et vulputate tortor. Nulla porta posuere odio, sit amet pulvinar nisl lobortis ac. Mauris porta placerat felis, non pellentesque justo scelerisque eu. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Phasellus ligula nunc, eleifend in tristique euismod, ultricies ac ex.
    Donec ultrices venenatis ligula ac blandit. Phasellus eget quam et magna facilisis condimentum in nec enim. Integer fermentum facilisis leo, a dignissim dui. Suspendisse aliquam molestie neque sed venenatis. Ut a malesuada nibh. Ut vel dui ante. Ut vestibulum consectetur tincidunt. Pellentesque eleifend, lacus volutpat auctor tincidunt, tortor eros accumsan ipsum, nec convallis massa mi sit amet ipsum. Vivamus eget metus est. Vestibulum molestie laoreet dictum. Vivamus ut nisi ut elit tempor elementum. Nulla suscipit justo a sem ornare, at aliquam neque pharetra. Curabitur id hendrerit arcu, sed ultricies urna. Donec hendrerit aliquet velit eu scelerisque. In malesuada dapibus ante. Pellentesque arcu magna, commodo quis nisl vel, volutpat cursus erat. </p> -->
  <div 
    class="box_preview {classState} {beforeState}"
    id="preview_1"
    bind:this={itemDiv}
    on:drop={(e)=> {console.log(e); console.log('box drop')}}></div>
</div>
