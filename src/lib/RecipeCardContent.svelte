<script lang="ts">
</script>

<div class="card-content">
  <slot name="card-content-list" />
  <div class="link-container">
    <slot name="link-container" />
  </div>
</div>

<style>
  .link-container {
    display: flex;
    justify-content: end;
  }
</style>
