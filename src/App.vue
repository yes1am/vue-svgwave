<template>
  <div id="app">
    <svg
      width="100%"
      height="100%"
      id="svg"
      :viewBox="`0 0 1440 ${height}`"
      xmlns="http://www.w3.org/2000/svg"
      className="transition duration-300 ease-in-out delay-150"
      v-html="getHtml()"
    />
  </div>
</template>

<script>

import { waveInit } from './core'

const wave = {
  height: 200,
  width: 400,
  // 有多少个波峰波谷
  segmentCount: 6,
  // 有多少层，最终等于 layerCount - 1
  layerCount: 5,
  variance: 0.75,
  strokeWidth: 0,
  fillColor: '#ff0080',
  strokeColor: 'none',
  animated: false,
}

export const OPACITY_ARR = ['44', '66', '88', 'ff']

export const MAX_WAVES = 4

export default {
  name: 'App',
  data() {
    return {
      xmlns: '',
      height: 0,
      html: ''
    }
  },
  mounted(){
    this.html = this.getHtml()
  },
  methods: {
    getHtml() {
      const gradient = true;
      const gradAngle = 270;
      const waveSvg = waveInit(wave)
      const gradColors = {
        colorOne: '#002bdc',
        colorTwo: '#32ded4',
      }
    const bgColor = '#ff0080';
    const { height, xmlns, path } = waveSvg.svg
    const num_waves = path.length
    const opac = OPACITY_ARR.slice(MAX_WAVES - num_waves)
    this.xmlns = xmlns;
    this.height = height;
    const results = path.map((p, index) => {
      const pathProps = []

        // if (p.animatedPath) {
        //   pathProps.push(`
        //    <style>{
        //   .path-${index}{
        //     animation:pathAnim-${index} 4s;
        //     animation-timing-function: linear;
        //     animation-iteration-count: infinite;
        //   }
        //   @keyframes pathAnim-${index}{
        //     0%{
        //       d: path("${p.d}");
        //     }
        //     25%{
        //       d: path("${p.animatedPath[0]}");
        //     }
        //     50%{
        //       d: path("${p.animatedPath[1]}");
        //     }
        //     75%{
        //       d: path("${p.animatedPath[2]}");
        //     }
        //     100%{
        //       d: path("${p.d}");
        //     }
        //   }}</style>`
        //   )
        // }

        console.log("gradient", gradient)
        if (gradient) {
          const anglePI = gradAngle * (Math.PI / 180)
          pathProps.push(
            `<defs>
              <LinearGradient
                id="gradient"
                x1=${Math.round(50 + Math.sin(anglePI) * 50) + '%'}
                y1=${Math.round(50 + Math.cos(anglePI) * 50) + '%'}
                x2=${Math.round(50 + Math.sin(anglePI + Math.PI) * 50) + '%'}
                y2=${Math.round(50 + Math.cos(anglePI + Math.PI) * 50) + '%'}
              >
                <stop
                  offset="5%"
                  stop-color=${`${gradColors.colorOne}${opac[index]}`}
                />
                <stop
                  offset="95%"
                  stop-color=${`${gradColors.colorTwo}${opac[index]}`}
                />
              </LinearGradient>
            </defs>`
          )
        }

        pathProps.push(`<path
            key="${index}"
            d="${p.d}"
            stroke="${p.strokeColor}"
            strokeWidth=${p.strokeWidth}
            fill=${gradient ? "url(#gradient)" : `${bgColor}${opac[index]}`}
            className='transition-all duration-300 ease-in-out delay-150 path-${index}'
          ></path>`
        )
        return pathProps.join("")
    })
    return results.join("")
  },
  },
  components: {
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
