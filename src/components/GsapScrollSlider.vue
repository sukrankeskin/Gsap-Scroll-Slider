<template>
  <div class="wrapp">
    <!-- header .header -->
    <header class="header">
      <div class="content">
        <h1 class="title">gsap scroll<span class="stroke">slider</span></h1>
      </div>
    </header>
    <!-- header .header END -->

    <!-- main .main -->
    <main class="main">
      <!-- section .section-slider -->
      <section class="section-slider gsap_slider">
        <div class="content">
          <div class="section__slider gsap_h">
            <!-- gsap__bl -->
            <div class="gsap__bl">
              <div class="gsap__inner">
                <div class="gsap__track">
                  <div
                    v-for="(item, index) in items"
                    :key="index"
                    class="gsap__item"
                  >
                    <span class="gsap__item-num"># {{ index + 1 }}</span>
                    <img :src="item.src" :alt="item.alt" />
                    <span class="gsap__item-img">
                      <img :src="item.src" :alt="item.alt" />
                    </span>
                  </div>
                </div>
              </div>
            </div>
            <!-- gsap__bl END -->
          </div>
        </div>
      </section>
      <!-- section .section END -->

      <section class="section-text">
        <div class="content">
          <p>
            Lorem ipsum dolor sit, amet consectetur adipisicing elit. Eos
            voluptatem fugit accusamus fuga vero quos, sint est laboriosam
            eveniet ea! Ducimus illum hic quas dolorem minima eius? Laboriosam
            iure deserunt totam sequi atque porro, dignissimos dolore sed
            laudantium. Ducimus modi corporis quaerat autem voluptates, quis,
            facere minima sequi reprehenderit itaque tempore illum perspiciatis
            delectus maiores! Aliquam placeat necessitatibus omnis voluptatem
            molestias repellat repudiandae quia ad quas, in accusantium, est
            enim esse quidem illo dolorem! Dolor, quam voluptatem! Facilis
            voluptatem tempore repellat accusamus quidem illo molestias odio,
            consequuntur rem mollitia dolores praesentium quisquam accusantium
            quo?
          </p>
        </div>
      </section>
    </main>
  </div>
</template>

<script>
import Lenis from "lenis";
import gsap from "gsap";
import ScrollTrigger from "gsap/ScrollTrigger";
import _ from "lodash";

export default {
  data() {
    return {
      items: [
        { src: require("../assets/img1.png"), alt: "1" },
        { src: require("../assets/img2.png"), alt: "2" },
        { src: require("../assets/img3.png"), alt: "3" },
        { src: require("../assets/img4.png"), alt: "4" },
        { src: require("../assets/img5.png"), alt: "5" },
        { src: require("../assets/img6.png"), alt: "6" },
        { src: require("../assets/img7.png"), alt: "7" },
        { src: require("../assets/img8.png"), alt: "8" },
      ],
      lenis: null,
    };
  },
  mounted() {
    this.initSmoothScroll();
    this.initScrollTrigger();
    window.addEventListener("resize", this.debouncedResize);
  },
  beforeDestroy() {
    window.removeEventListener("resize", this.debouncedResize);
  },
  methods: {
    initSmoothScroll() {
      this.lenis = new Lenis({
        duration: 1.2,
      });

      this.lenis.on("scroll", (e) => {
        console.log(e);
      });

      const raf = (time) => {
        this.lenis.raf(time);
        requestAnimationFrame(raf);
      };
      requestAnimationFrame(raf);

      this.lenis.on("scroll", ScrollTrigger.update);

      gsap.ticker.add((time) => {
        this.lenis.raf(time * 1000);
      });
    },
    initScrollTrigger() {
      gsap.registerPlugin(ScrollTrigger);

      let gsapBl = this.$el.querySelector(".gsap__bl").offsetWidth;
      let gsapTrack = this.$el.querySelector(".gsap__track").offsetWidth;
      let scrollSliderTransform = gsapTrack - gsapBl;

      gsap.to(".gsap__track", {
        scrollTrigger: {
          trigger: ".gsap_slider",
          start: "center center",
          end: () => "+=" + gsapTrack,
          pin: true,
          scrub: true,
        },
        x: "-=" + scrollSliderTransform + "px",
      });
    },
    onWindowResize() {
      console.log("Window resized!");
      location.reload();
    },
  },
  computed: {
    debouncedResize() {
      return _.debounce(this.onWindowResize, 500);
    },
  },
};
</script>

<style scoped>
@import url("../assets/normalize.css");
@import url("../assets/style.css");
</style>
