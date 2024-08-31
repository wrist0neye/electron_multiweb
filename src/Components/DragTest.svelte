<script>
</script>

<style>
  .container{
    width:60vw;
    height:480px;
    border-radius: 8px;
    background-color: dimgray;

    display: flex;
    flex-direction: row;
    justify-content: space-around;
    align-items: center;
  }
  .small_box{
    width : 120px;
    height : 120px;
    border-radius: 5px;
    background-color: blueviolet;
  }
  .item{
    width:60px;
    height:60px;
    border-radius: 30px;
    background-color: aquamarine;
  }  
</style>

<div class="container">
  <div class="small_box" on:dragover="{(e)=>{console.log(e.clientX, e.clientY, e.offsetX, e.offsetY); console.log('item over smallbox1')}}">
    <!-- <div class="item" on:dragstart="{(e) => {console.log(e); console.log('item start moving..')}}"></div> -->
    <div class="item"></div>
  </div>
  <div class="small_box" on:dragover="{(e)=>{console.log(e.clientX, e.clientY, e.offsetX, e.offsetY); console.log('item over smallbox2')}}"></div>
</div>