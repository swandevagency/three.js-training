<template>
  <div class="nuxt-app-wrapper">
    <div
      class="landing-image-wrapper"
      @mousemove="rotateLandingImage"
      @mouseleave="resetLandingImage"
      ref="landingImageContainer"
    >
      <div class="landing-title-wrapper">
        <img src="/Polygon patterns.png" alt="" />
        <h2>Welcome to</h2>
        <h1>Three js Training</h1>
        <p>This site is based on Nuxt<br />and three js framework</p>
      </div>
      <div class="landing-illustrations-wrapper-animator">
        <div
          class="landing-illustrations-wrapper"
          ref="landingIllustrationImages"
        >
          <img src="/illustration-cube.png" id="1" alt="" />
          <img src="/illustration-ball.png" id="2" alt="" />
          <img src="/illustration-pyramid.png" id="3" alt="" />
        </div>
      </div>
    </div>
    <div class="browse-objects-wrapper">
      <h2>Browse objects</h2>
      <ObjectItem
        v-for="objectItem in objectItems"
        :key="objectItem.Id"
        :objectItem="objectItem"
      />
    </div>
    <WhatIsThreeJs />
  </div>
</template>

<script>
import * as THREE from "three";
import ObjectItem from "../components/ObjectItem.vue";
import WhatIsThreeJs from "../components/WhatIsThreeJs.vue";
export default {
  data() {
    return {
      objectItems: [
        {
          Id: 0,
          objectType: "Cube",
          itemTitle: "Mega Cube",
          itemSubtitle:
            "This is a mega cube with texture and normal map attached to it",
        },
        {
          Id: 1,
          objectType: "Sphere",
          itemTitle: "Mega Sphere",
          itemSubtitle:
            "This is a mega sphere with texture and normal map attached to it",
        },
        {
          Id: 2,
          objectType: "Pyramid",
          itemTitle: "Mega Pyramid",
          itemSubtitle:
            "This is a mega pyramid with texture and normal map attached to it",
        },
      ],
    };
  },
  components: {
    ObjectItem,
    WhatIsThreeJs,
  },
  methods: {
    rotateLandingImage(event) {
      const cursor = {
        positionX:
          ((event.pageX - this.$refs.landingImageContainer.offsetLeft) /
            this.$refs.landingImageContainer.clientWidth -
            0.5) *
          0.1,
        positionY:
          -(
            (event.pageY - this.$refs.landingImageContainer.offsetTop) /
              this.$refs.landingImageContainer.clientHeight -
            0.5
          ) * 0.1,
      };
      this.$refs.landingImageContainer.style.transform = `perspective(1em) rotateX(${cursor.positionY}deg) rotateY(${cursor.positionX}deg)`;
      this.$refs.landingImageContainer.style.setProperty(
        "--shine-x",
        `${event.pageX - 200 - this.$refs.landingImageContainer.offsetLeft}px`
      );
      this.$refs.landingImageContainer.style.setProperty(
        "--shine-y",
        `${event.pageY - 200 - this.$refs.landingImageContainer.offsetTop}px`
      );
      this.$refs.landingIllustrationImages.style.transform = `perspective(1em) translateX(${
        cursor.positionX * -800
      }px) translateY(${cursor.positionY * 800}px)`;
    },
    resetLandingImage() {
      this.$refs.landingImageContainer.style.transform = `perspective(1em) rotateY(0deg) rotateX(0deg)`;
      this.$refs.landingIllustrationImages.style.transform = `perspective(1em) translateX(0px) translateY(0px)`;
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Poppins:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap");

:root {
  --border: 1px solid #a4a4a4;
  --border-radius: 10px;
  --gradient-background: linear-gradient(-90deg, #ffe53b 0%, #ff2525 100%);
  --font-family: Poppins;
  --landing-image-box-shadow: 0px 8px 20px rgba(0, 0, 0, 0.22);
  --text-shadow: 0px 3px 6px rgba(0, 0, 0, 0.16);
  --object-item-drop-shadow: 0px 12px 60px rgba(0, 0, 0, 0.16);
  --illustration-drop-shadow: drop-shadow(0px 3px 6px rgba(0, 0, 0, 0.16));
  --text-color: #3d3d3d;
  --footer-information-background: linear-gradient(
    -90deg,
    #53556c 0%,
    #1d2135 100%
  );
  --shine-background: radial-gradient(
    circle,
    rgba(255, 255, 255, 1),
    rgba(255, 255, 255, 0) 50%
  );
  --dark-shine-background: radial-gradient(
    circle,
    rgba(0, 0, 0, 1) 0%,
    rgba(255, 255, 255, 0) 50%
  );
}

::selection {
  color: white;
  background-color: #897ded;
}

* {
  transition: all 0.3s cubic-bezier(0, 0, 0.58, 1);
}

body {
  padding: 50px 140px;
  margin: 0;
  font-family: Poppins;
}

/* .nuxt-app-wrapper {
  margin-top: 50px;
} */

.landing-image-wrapper {
  display: flex;
  justify-content: space-between;
  background: var(--gradient-background);
  border-radius: var(--border-radius);
  color: white;
  padding: 120px 60px;
  position: relative;
  box-shadow: var(--landing-image-box-shadow);
  overflow: hidden;
}
.landing-image-wrapper::after {
  transition: opacity 0.5s cubic-bezier(0, 0, 0.58, 1);
  content: "";
  position: absolute;
  width: 400px;
  height: 400px;
  top: var(--shine-y);
  left: var(--shine-x);
  background: var(--shine-background);
  opacity: 0;
}
.landing-image-wrapper:hover::after {
  opacity: 0.2;
}
.landing-illustrations-wrapper-animator {
  animation: float-svgs 5s ease-in-out infinite alternate-reverse both;
}
.landing-illustrations-wrapper > img[id="1"] {
  filter: var(--illustration-drop-shadow);
  transform: translate(-45px, -20px);
}
.landing-illustrations-wrapper > img[id="2"] {
  filter: var(--illustration-drop-shadow);
  transform: translate(-30px, 120px);
}
.landing-illustrations-wrapper > img[id="3"] {
  filter: var(--illustration-drop-shadow);
  transform: translate(-35px, -65px);
}
.landing-title-wrapper {
  text-shadow: var(--text-shadow);
}
.landing-title-wrapper > img {
  mix-blend-mode: soft-light;
  position: absolute;
  top: 0;
  left: 0;
  height: 100%;
  width: 400px;
  object-position: left;
  object-fit: contain;
}
.landing-title-wrapper > h2 {
  margin: 0;
  font-weight: 400;
}
.landing-title-wrapper > h1 {
  margin: 0;
  font-weight: 400;
  font-size: 70px;
}
.landing-title-wrapper > p {
  font-size: 20px;
  font-weight: 300;
}
.browse-objects-wrapper {
  margin-top: 64px;
  display: grid;
  grid-gap: 20px;
  grid-template-columns: repeat(3, 1fr);
}
.browse-objects-wrapper > h2 {
  grid-column: 1 / 4;
  color: var(--text-color);
}

/* #region Animations */
@keyframes float-svgs {
  0% {
    transform: translateY(25px);
  }
  100% {
    transform: translateY(-25px);
  }
}
/* #endregion */
</style>