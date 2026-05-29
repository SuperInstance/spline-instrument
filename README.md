# spline-instrument

**A graphing calculator as a musical instrument. Sculpt waveforms, hear them as sound. The spline IS the sound.**

A single-file HTML/JS application — zero dependencies, no build step. Open `index.html` in any browser.

## What it does

Spline Instrument lets you sculpt waveforms by placing and dragging control points on a canvas. The resulting curve is converted to audio in real-time via the Web Audio API. Three modes transform your spline into different waveforms:

1. **Spline Sculpt** — Cubic Catmull-Rom interpolation through control points. The curve shape directly becomes the waveform.
2. **Fourier Paint** — Control points define harmonics (x = frequency, y = amplitude). The resulting Fourier series becomes the waveform.
3. **Laplacian Wave** — Control points are coupled oscillators connected to neighbors. Wave propagation through the graph produces evolving, organic waveforms.

## How to use

1. Open `index.html` in a browser
2. Click on the canvas to place control points
3. Drag points to sculpt the waveform
4. Right-click to delete a point
5. Click **Play Sound** to hear the waveform at 220 Hz
6. Switch modes with the dropdown

### Controls

- **Clear** — Remove all points
- **Add Harmonic** — Add a random control point
- **Play Sound** — Toggle audio playback
- **Toggle Trail** — Show/hide the scanning dot trail
- **Mode selector** — Switch between Spline Sculpt, Fourier Paint, and Laplacian Wave

### Display

- **freq** — Current frequency at the scanning dot
- **CR** — Conservation ratio (spectral flatness) of the waveform
- **nodes** — Number of control points
- **mode** — Active mode

## The math

### Catmull-Rom Spline

```javascript
function catmullRom(p0, p1, p2, p3, t) {
  const t2 = t*t, t3 = t2*t;
  return 0.5 * ((2*p1) + (-p0+p2)*t + (2*p0-5*p1+4*p2-p3)*t2 + (-p0+3*p1-3*p2+p3)*t3);
}
```

### Conservation Ratio

The CR is computed from the DFT of the waveform — the ratio of the second-largest spectral component to the largest. A high CR means the waveform has a complex spectrum; low CR means it's dominated by a single frequency.

### Audio Synthesis

The spline waveform is converted to a custom periodic wave via `AudioContext.createPeriodicWave()`, computing Fourier coefficients from the spline's shape. This gives a smooth, continuous tone that exactly matches the visual waveform.

## Architecture

Everything lives in `index.html`:
- Canvas rendering with glow effects and gradient waveforms
- Catmull-Rom spline interpolation
- Fourier series synthesis from control points
- Laplacian wave propagation (coupled spring-mass system)
- DFT-based CR computation
- Web Audio API integration

## Running

```bash
# Just open it
open index.html
# Or serve it locally
python3 -m http.server 8000
```

Part of the [SuperInstance OpenConstruct](https://github.com/SuperInstance/OpenConstruct) ecosystem.

## License

MIT
