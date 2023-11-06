<template>
    <div class="section">
      <div class="container">
        <div class="columns is-centered">
          <div class="column is-one-third">
    <input type="color" v-model="selectedColor" @input="updateColorFromHex" style="width: 100%; height: 350px;"/>
        </div>
          <div class="column is-one-third">
            <div class="color-info">
              <div class="color-codes">
                <div class="field">
                  <label class="label">HEX:</label>
                  <div class="control">
                    <input class="input" v-model="hex" @input="updateColorFromHex" readonly />
                  </div>
                </div>
                <div class="field">
                  <label class="label">RGB:</label>
                  <div class="control">
                    <input class="input" v-model="rgb" @input="updateColorFromRGB" readonly />
                  </div>
                </div>
                <div class="field">
                  <label class="label">CMYK:</label>
                  <div class="control">
                    <input class="input" v-model="cmyk" @input="updateColorFromCMYK" readonly />
                  </div>
                </div>
                <div class="field">
                  <label class="label">HSB:</label>
                  <div class="control">
                    <input class="input" v-model="hsb" @input="updateColorFromHSB" readonly />
                  </div>
                </div>
                <div class="field">
                  <label class="label">HUE:</label>
                  <div class="control">
                    <input class="input" v-model="hue" @input="updateColorFromHSL" readonly />
                  </div>
                </div>
                <div class="field">
                  <label class="label">HSL:</label>
                  <div class="control">
                    <input class="input" v-model="hsl" @input="updateColorFromHSL" readonly />
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        selectedColor: "#00d1b2",
        rgb: this.calculateRGB("#00d1b2"),
        cmyk: this.calculateCMYK("#00d1b2"),
        hex: "#00d1b2",
        hsb: this.calculateHSB("#00d1b2"),
        hue: this.calculateHue("#00d1b2"),
        hsl: this.calculateHSL("#00d1b2"),
      };
    },
    methods: {
      cmykToHex(cmyk) {
        const [c, m, y, k] = cmyk.split(",").map(Number);
        const r = 255 * (1 - Math.min(1, c * (1 - k) + k));
        const g = 255 * (1 - Math.min(1, m * (1 - k) + k));
        const b = 255 * (1 - Math.min(1, y * (1 - k) + k));
        const hex = `#${Math.round(r).toString(16).padStart(2, "0")}${Math.round(g).toString(16).padStart(2, "0")}${Math.round(b).toString(16).padStart(2, "0")}`;
        return hex.toUpperCase();
      },
      updateColorFromHex() {
        this.hex = this.selectedColor;
        this.rgb = this.calculateRGB(this.selectedColor);
        this.cmyk = this.calculateCMYK(this.selectedColor);
        this.hsb = this.calculateHSB(this.selectedColor);
        this.hue = this.calculateHue(this.selectedColor);
        this.hsl = this.calculateHSL(this.selectedColor);
      },
      updateColorFromRGB() {
        this.selectedColor = this.rgbToHex(this.rgb);
        this.cmyk = this.calculateCMYK(this.selectedColor);
        this.hsb = this.calculateHSB(this.selectedColor);
        this.hue = this.calculateHue(this.selectedColor);
        this.hsl = this.calculateHSL(this.selectedColor);
      },
      updateColorFromCMYK() {
        this.selectedColor = this.cmykToHex(this.cmyk);
        this.rgb = this.calculateRGB(this.selectedColor);
        this.hsb = this.calculateHSB(this.selectedColor);
        this.hue = this.calculateHue(this.selectedColor);
        this.hsl = this.calculateHSL(this.selectedColor);
      },
      updateColorFromHSB() {
        this.selectedColor = this.hsbToHex(this.hsb);
        this.rgb = this.calculateRGB(this.selectedColor);
        this.cmyk = this.calculateCMYK(this.selectedColor);
        this.hue = this.calculateHue(this.selectedColor);
        this.hsl = this.calculateHSL(this.selectedColor);
      },
      updateColorFromHSL() {
        this.selectedColor = this.hslToHex(this.hsl);
        this.rgb = this.calculateRGB(this.selectedColor);
        this.cmyk = this.calculateCMYK(this.selectedColor);
        this.hsb = this.calculateHSB(this.selectedColor);
        this.hue = this.calculateHue(this.selectedColor);
      },
      calculateRGB(hex) {
        const r = parseInt(hex.slice(1, 3), 16);
        const g = parseInt(hex.slice(3, 5), 16);
        const b = parseInt(hex.slice(5, 7), 16);
        return `${r},${g},${b}`;
      },
      calculateCMYK(hex) {
        const [r, g, b] = this.calculateRGB(hex).split(',').map(Number);
        const c = 1 - r / 255;
        const m = 1 - g / 255;
        const y = 1 - b / 255;
        const k = Math.min(c, m, y);
        return `${Math.round(c * 100)},${Math.round(m * 100)},${Math.round(y * 100)},${Math.round(k * 100)}`;
      },
      rgbToHex(rgb) {
        const [r, g, b] = rgb.split(',').map(Number);
        const hex = `#${r.toString(16).padStart(2, '0')}${g.toString(16).padStart(2, '0')}${b.toString(16).padStart(2, '0')}`;
        return hex.toUpperCase();
      },
      calculateHSB(hex) {
        const [r, g, b] = this.calculateRGB(hex).split(',').map(Number);
        const max = Math.max(r, g, b);
        const min = Math.min(r, g, b);
        const delta = max - min;
        let h = 0;
        if (delta !== 0) {
          if (max === r) {
            h = (60 * ((g - b) / delta) + 360) % 360;
          } else if (max === g) {
            h = (60 * ((b - r) / delta) + 120);
          } else {
            h = (60 * ((r - g) / delta) + 240);
          }
        }
        const s = max === 0 ? 0 : (delta / max) * 100;
        const v = (max / 255) * 100;
        return `${Math.round(h)},${Math.round(s)},${Math.round(v)}`;
      },
      hsbToHex(hsb) {
        const [h, s, v] = hsb.split(',').map(Number);
        const c = (s / 100) * (v / 100);
        const x = c * (1 - Math.abs(((h / 60) % 2) - 1));
        const m = (v / 100) - c;
        let r, g, b;
        if (h >= 0 && h < 60) {
          r = c;
          g = x;
          b = 0;
        } else if (h >= 60 && h < 120) {
          r = x;
          g = c;
          b = 0;
        } else if (h >= 120 && h < 180) {
          r = 0;
          g = c;
          b = x;
        } else if (h >= 180 && h < 240) {
          r = 0;
          g = x;
          b = c;
        } else if (h >= 240 && h < 300) {
          r = x;
          g = 0;
          b = c;
        } else {
          r = c;
          g = 0;
          b = x;
        }
        r = Math.round((r + m) * 255);
        g = Math.round((g + m) * 255);
        b = Math.round((b + m) * 255);
        const hex = `#${r.toString(16).padStart(2, '0')}${g.toString(16).padStart(2, '0')}${b.toString(16).padStart(2, '0')}`;
        return hex.toUpperCase();
      },
      calculateHue(hex) {
        const [r, g, b] = this.calculateRGB(hex).split(',').map(Number);
        const max = Math.max(r, g, b);
        const min = Math.min(r, g, b);
        const delta = max - min;
        let hue = 0;
        if (delta !== 0) {
          if (max === r) {
            hue = (60 * ((g - b) / delta) + 360) % 360;
          } else if (max === g) {
            hue = (60 * ((b - r) / delta) + 120);
          } else {
            hue = (60 * ((r - g) / delta) + 240);
          }
        }
        return Math.round(hue);
      },
      calculateHSL(hex) {
        const [r, g, b] = this.calculateRGB(hex).split(',').map(Number);
        const max = Math.max(r, g, b);
        const min = Math.min(r, g, b);
        const delta = max - min;
        const lightness = (max + min) / 2;
        let saturation = 0;
        if (delta !== 0) {
          saturation = (delta / (1 - Math.abs(2 * lightness - 1))) * 100;
        }
        return `${this.calculateHue(hex)},${Math.round(saturation)},${Math.round(lightness)}`;
      },
      hslToHex(hsl) {
        const [h, s, l] = hsl.split(',').map(Number);
        const c = (1 - Math.abs(2 * (l / 100) - 1)) * (s / 100);
        const x = c * (1 - Math.abs(((h / 60) % 2) - 1));
        const m = (l / 100) - c / 2;
        let r, g, b;
        if (h >= 0 && h < 60) {
          r = c;
          g = x;
          b = 0;
        } else if (h >= 60 && h < 120) {
          r = x;
          g = c;
          b = 0;
        } else if (h >= 120 && h < 180) {
          r = 0;
          g = c;
          b = x;
        } else if (h >= 180 && h < 240) {
          r = 0;
          g = x;
          b = c;
        } else if (h >= 240 && h < 300) {
          r = x;
          g = 0;
          b = c;
        } else {
          r = c;
          g = 0;
          b = x;
        }
        r = Math.round((r + m) * 255);
        g = Math.round((g + m) * 255);
        b = Math.round((b + m) * 255);
        const hex = `#${r.toString(16).padStart(2, '0')}${g.toString(16).padStart(2, '0')}${b.toString(16).padStart(2, '0')}`;
      return hex.toUpperCase();
    },
  },
};
</script>

<style scoped>
.color-info {
  text-align: center;
}
.field {
  margin-bottom: 1rem;
}
</style>
