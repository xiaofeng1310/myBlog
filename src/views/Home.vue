<template>
    <div class="home">
        <canvas id="canvas" ref="canvas"></canvas>
        <div class="title">小风博客</div>
        <div class="lists">
            <router-link to="/docs">docs</router-link>
        </div>
    </div>
</template>

<script>
import { BackgroundRenderer, loadImage, isWebGLSupported, EffectType, TransitionType, Easings, SlideDirection} from "midori-bg";
import Bg from '../assets/images/home/bg.jpg';
export default {
    name: "Home",
    mounted () {
        const canvas = this.$refs.canvas
        if (isWebGLSupported()) {
            const renderer = new BackgroundRenderer(canvas);
            loadImage(Bg)
                // set background
                .then((image) => {
                    renderer.setBackground(image);
                    const {particles, camera} = renderer.background
                    // generate two named groups of particles in the background.
                    particles.generate([
                    {
                        name: 'small',
                        amount: 200,
                        maxSize: 5,
                        maxOpacity: 0.8,
                        minGradient: 0.75,
                        maxGradient: 1.0,
                        color: 0xffffff,
                        smoothing: 0.6,
                    }
                    ]);
                    // move the particles by a distance and angle in degrees with a transition.
                    particles.move('small', { distance: 0.5, angle: 230 }, { duration: 5, loop: true });
                    // sway the particles up to a given distance with a transition.
                    particles.sway('small', { x: 0.025, y: 0.025 }, { duration: 1.5, easing: Easings.Sinusoidal.InOut, loop: true });
                    // sway the camera around its center with a transition.
                    // x - up to 10% of the background width away from the center
                    // y - up to 5% of the background height away from the center
                    // z - up to 2% of the maximum zoom from the center
                    // zr - up to 1 degree of rotation from the center
                    camera.sway({ x: 0.1, y: 0.05, z: 0.02, zr: 1 }, {
                        duration: 1.5,
                        easing: Easings.Quadratic.InOut,
                        loop: true,
                    });
                })
                // handle errors
                .catch(err => console.error(err));
        }
    }
};
</script>
<style lang="less">
.home {
    position: relative;
    height: 100%;
    font-size: 36px;
    #canvas {
        position: absolute;
        top: 0;
        left: 0;
        width: 100%;
        height: 100%;
        z-index: -1;
    }
    // background: url("../assets/home/bg.jpg") no-repeat;
    // background-size: 100% 100%;
}
</style>
