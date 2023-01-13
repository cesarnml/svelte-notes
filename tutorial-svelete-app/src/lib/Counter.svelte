<script lang="ts">
  const m = {
    x: 0,
    y: 0,
  }
</script>

<button
  on:mousemove={(e) => {
    m.x = e.clientX
    m.y = e.clientY
  }}
>
  the mouse position is ({m.x}, {m.y})
</button>
<div>base</div>
