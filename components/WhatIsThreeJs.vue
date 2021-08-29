<template>
  <div class="what-is-three-js-wrapper">
    <div class="descriptions-wrapper">
      <div class="what-is-three-js-description-wrapper">
        <h2>What is three js?</h2>
        <p>
          Three.js is a cross-browser JavaScript library and application
          programming interface used to create and display animated 3D computer
          graphics in a web browser using WebGL. The source code is hosted in a
          repository on GitHub.
        </p>
      </div>
      <div class="websites-made-with-three-js-description-wrapper">
        <h2>Websites made with three js</h2>
        <ul>
          <li><a href="#">Orano experience</a></li>
          <li><a href="#">Prior</a></li>
          <li><a href="#">Citrix x Redbull</a></li>
          <li><a href="#">Zenly</a></li>
        </ul>
      </div>
    </div>
    <footer>
      <div class="left-illustrations-wrapper">
        <img src="/illustration-cube.png" id="4" alt="" />
        <img src="/illustration-ball.png" id="5" alt="" />
        <img src="/illustration-pyramid.png" id="6" alt="" />
      </div>
      <div
        class="footer-information-wrapper"
        ref="footerInformation"
        @mousemove="rotateFooterInformation"
        @mouseleave="resetFooterInformation"
      >
        <div class="polygon-pattern-images-wrapper">
          <img src="/Polygon patterns.png" alt="" />
          <img
            src="/Polygon patterns.png"
            alt=""
            style="transform: scaleX(-1)"
          />
        </div>
        <h3>Programmed by</h3>
        <p>Mega Red</p>
        <a href="#">GitHub profile</a>
      </div>
      <div class="right-illustrations-wrapper">
        <img src="/illustration-pyramid.png" id="7" alt="" />
        <img src="/illustration-ball.png" id="8" alt="" />
        <img src="/illustration-cube.png" id="9" alt="" />
      </div>
    </footer>
  </div>
</template>

<script>
export default {
  methods: {
    rotateFooterInformation(event) {
      const cursor = {
        positionX:
          ((event.pageX - this.$refs.footerInformation.offsetLeft) /
            this.$refs.footerInformation.clientWidth -
            0.5) *
          0.1,
        positionY:
          -(
            (event.pageY - this.$refs.footerInformation.offsetTop) /
              this.$refs.footerInformation.clientHeight -
            0.5
          ) * 0.1,
      };
      this.$refs.footerInformation.style.transform = `perspective(1em) rotateX(${cursor.positionY}deg) rotateY(${cursor.positionX}deg)`;
      this.$refs.footerInformation.style.setProperty(
        "--shine-x",
        `${event.pageX - 200 - this.$refs.footerInformation.offsetLeft}px`
      );
      this.$refs.footerInformation.style.setProperty(
        "--shine-y",
        `${event.pageY - 200 - this.$refs.footerInformation.offsetTop}px`
      );
    },
    resetFooterInformation() {
      this.$refs.footerInformation.style.transform = `perspective(1em) rotateY(0deg) rotateX(0deg)`;
    },
  },
};
</script>

<style>
.what-is-three-js-wrapper {
  margin-top: 84px;
}
.descriptions-wrapper {
  display: flex;
  justify-content: space-between;
}
.what-is-three-js-description-wrapper,
.websites-made-with-three-js-description-wrapper {
  flex-basis: calc(50% - 40px);
}
.what-is-three-js-description-wrapper > h2,
.websites-made-with-three-js-description-wrapper > h2 {
  margin: 0;
  color: var(--text-color);
}
.what-is-three-js-description-wrapper > p {
  margin-top: 45px;
  color: var(--text-color);
}
.websites-made-with-three-js-description-wrapper > ul {
  list-style: none;
  margin-top: 40px;
  padding: 0;
}
.websites-made-with-three-js-description-wrapper > ul > li > a {
  color: var(--text-color);
}
footer {
  margin: 84px 0 84px;
  display: flex;
  justify-content: space-between;
}
.left-illustrations-wrapper,
.footer-information-wrapper,
.right-illustrations-wrapper {
  flex-basis: calc((100% / 3) - 45px);
}
.left-illustrations-wrapper,
.right-illustrations-wrapper {
  animation: float-svgs 5s ease-in-out infinite alternate-reverse both;
}
.left-illustrations-wrapper > img[id="4"] {
  filter: var(--illustration-drop-shadow);
  transform: translate(-35px, -65px);
}
.left-illustrations-wrapper > img[id="5"] {
  filter: var(--illustration-drop-shadow);
  transform: translate(-30px, 120px);
}
.left-illustrations-wrapper > img[id="6"] {
  filter: var(--illustration-drop-shadow);
  transform: translate(30px, -80px);
}
.right-illustrations-wrapper {
  animation: float-svgs 5s ease-in-out infinite alternate-reverse both;
}
.right-illustrations-wrapper > img[id="7"] {
  filter: var(--illustration-drop-shadow);
  transform: translate(35px, 65px);
}
.right-illustrations-wrapper > img[id="8"] {
  filter: var(--illustration-drop-shadow);
  transform: translate(30px, -120px);
}
.right-illustrations-wrapper > img[id="9"] {
  filter: var(--illustration-drop-shadow);
  transform: translate(-30px, 80px);
}
.footer-information-wrapper {
  position: relative;
  color: white;
  padding: 47px 69px;
  display: flex;
  flex-direction: column;
  align-items: center;
  box-sizing: border-box;
  overflow: hidden;
  background: var(--footer-information-background);
  border-radius: var(--border-radius);
  box-shadow: 0px 8px 20px rgba(0, 0, 0, 0.22);
}
.footer-information-wrapper::after {
  transition: opacity 0.5s cubic-bezier(0, 0, 0.58, 1);
  content: "";
  position: absolute;
  width: 400px;
  height: 400px;
  pointer-events: none;
  top: var(--shine-y);
  left: var(--shine-x);
  background: var(--shine-background);
  opacity: 0;
}
.footer-information-wrapper:hover::after {
  opacity: 0.05;
}
.footer-information-wrapper > h3 {
  font-size: 18px;
  font-weight: 400;
  margin: 0;
}
.footer-information-wrapper > p {
  font-weight: 200;
  margin: 0;
}
.footer-information-wrapper > a {
  color: white;
  margin: 35px 0 0;
}
.polygon-pattern-images-wrapper {
  display: flex;
  justify-content: space-between;
  position: absolute;
  height: 100%;
  width: 100%;
  top: 0;
  left: 0;
}
.polygon-pattern-images-wrapper > img {
  mix-blend-mode: soft-light;
  /* width: 400px; */
  /* transform: translateX(-40px); */
  object-position: left;
  object-fit: contain;
}
</style>