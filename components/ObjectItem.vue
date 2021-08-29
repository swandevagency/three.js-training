<template>
  <div
    class="object-item-wrapper"
    ref="objectItem"
    @mousemove="rotateObjectItem"
    @mouseleave="resetObjectItem"
  >
    <canvas class="object-item-canvas" ref="renderCanvas"></canvas>
    <h2>{{ objectItem.itemTitle }}</h2>
    <p>{{ objectItem.itemSubtitle }}</p>
  </div>
</template>

<script>
export default {
  props: {
    objectItem: Object,
  },
  methods: {
    rotateObjectItem(event) {
      const cursor = {
        positionX:
          ((event.pageX - this.$refs.objectItem.offsetLeft) /
            this.$refs.objectItem.clientWidth -
            0.5) *
          0.1,
        positionY:
          -(
            (event.pageY - this.$refs.objectItem.offsetTop) /
              this.$refs.objectItem.clientHeight -
            0.5
          ) * 0.1,
      };
      this.$refs.objectItem.style.transform = `perspective(1em) rotateX(${cursor.positionY}deg) rotateY(${cursor.positionX}deg) scale(1.02)`;
      this.$refs.objectItem.style.setProperty(
        "--shine-x",
        `${event.pageX - 200 - this.$refs.objectItem.offsetLeft}px`
      );
      this.$refs.objectItem.style.setProperty(
        "--shine-y",
        `${event.pageY - 5 - this.$refs.objectItem.offsetTop}px`
      );
    },
    resetObjectItem() {
      this.$refs.objectItem.style.transform = `perspective(1em) rotateY(0deg) rotateX(0deg)`;
    },
  },
};
</script>

<style>
/* .object-item-wrapper {

} */
.object-item-wrapper {
  text-align: center;
  border: var(--border);
  overflow: hidden;
  border-radius: var(--border-radius);
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.16);
  padding: 24px;
}
.object-item-wrapper:hover {
  box-shadow: var(--object-item-drop-shadow);
}
.object-item-wrapper::after {
  transition: opacity 0.5s cubic-bezier(0, 0, 0.58, 1);
  content: "";
  position: absolute;
  width: 400px;
  height: 10px;
  top: var(--shine-y);
  left: var(--shine-x);
  background: var(--dark-shine-background);
  opacity: 0;
}
.object-item-wrapper:hover::after {
  opacity: 0.1;
}
.object-item-wrapper > h2 {
  margin: 20px 0 30px;
  font-weight: 300;
  color: var(--text-color);
}
.object-item-wrapper > p {
  color: var(--text-color);
  margin: 0;
}
.object-item-canvas {
  border: var(--border);
  width: 100%;
  height: 300px;
  border-radius: var(--border-radius);
}
</style>