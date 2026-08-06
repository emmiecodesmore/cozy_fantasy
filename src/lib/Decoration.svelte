<script>
  export let src;
  export let top = '0';
  export let left = '0';
  export let size = '140px';
  export let opacity = 0.45;
  export let rotate = '0deg';
</script>

<img
  {src}
  alt=""
  class="decoration"
  style="top:{top}; left:{left}; width:{size}; opacity:{opacity}; transform: rotate({rotate});"
/>

<style>
  .decoration {
    position: absolute;
    pointer-events: none;
    z-index: 0;
  }
</style>