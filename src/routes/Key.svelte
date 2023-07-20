<script>
export let pressed = false;
export let key = "A"


</script>
<div class="key-holder" class:pressed={pressed} on:pointerdown={()=>{pressed = true}}
                                              on:pointerup={()=>{pressed=false}}
 >
    {key}
</div>
<style>

.key-holder{
    width: 50px;
    height: 60px;
    display: flex;
    background-color: #e0e0e0;
    font-family: 'Source Code Pro';
    color: #000;
    justify-content: center;
    align-items: center;
    font-size: 22px;
    border-radius: 6px;
    border-bottom-left-radius: 9px;
    border-bottom-right-radius: 9px;
    border-bottom: 3.5px solid #bdbdbd;
    border-top: 2px solid #f5f5f5;
    /* box-shadow: -2px 2px 1px #929292; */
}

.pressed{
    background-color: #99999999;
    border-bottom: 0px solid #f5f5f5;
    border-top: 3px solid #f5f5f5;
    /* box-shadow: -1px 1px 1px #929292; */
}

</style>