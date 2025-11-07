import React, { useState, useEffect, useRef } from 'react';
import { Waves, Activity, Zap, Wind, Eye, Minus, Plus } from 'lucide-react';

export default function SelfAsProcess() {
  const [mode, setMode] = useState('pattern');
  const [flowRate, setFlowRate] = useState(50);
  const [particles, setParticles] = useState([]);
  const [wavePhase, setWavePhase] = useState(0);
  const canvasRef = useRef(null);

  // Initialize flowing particles
  useEffect(() => {
    const newParticles = Array.from({ length: 30 }, (_, i) => ({
      id: i,
      x: Math.random() * 100,
      y: 20 + Math.random() * 60,
      vx: 0.2 + Math.random() * 0.3,
      vy: (Math.random() - 0.5) * 0.1,
      size: 2 + Math.random() * 4,
      hue: 180 + Math.random() * 80,
      age: 0,
      maxAge: 100 + Math.random() * 100,
    }));
    setParticles(newParticles);
  }, []);

  // Animate particles as flowing process
  useEffect(() => {
    const interval = setInterval(() => {
      const speed = flowRate / 50;
      
      setWavePhase(prev => (prev + 0.05 * speed) % (Math.PI * 2));
      
      setParticles(prev => prev.map(p => {
        let newP = {
          ...p,
          x: p.x + p.vx * speed,
          y: p.y + p.vy * speed + Math.sin(p.x / 10 + wavePhase) * 0.2,
          age: p.age + 1,
        };

        // Particles flow and regenerate (becoming/unbecoming)
        if (newP.x > 100 || newP.age > newP.maxAge) {
          newP = {
            ...newP,
            x: -5,
            y: 20 + Math.random() * 60,
            age: 0,
            maxAge: 100 + Math.random() * 100,
            hue: 180 + Math.random() * 80,
          };
        }

        return newP;
      }));
    }, 50);

    return () => clearInterval(interval);
  }, [flowRate, wavePhase]);

  // Draw flow field on canvas
  useEffect(() => {
    const canvas = canvasRef.current;
    if (!canvas) return;
    
    const ctx = canvas.getContext('2d');
    const width = canvas.width;
    const height = canvas.height;

    const animate = () => {
      ctx.fillStyle = 'rgba(0, 0, 0, 0.05)';
      ctx.fillRect(0, 0, width, height);

      // Draw flow lines showing the pattern structure
      ctx.strokeStyle = `rgba(139, 92, 246, ${0.1 + flowRate / 200})`;
      ctx.lineWidth = 1;

      for (let y = 0; y < height; y += 20) {
        ctx.beginPath();
        for (let x = 0; x < width; x += 5) {
          const wave = Math.sin(x / 30 + wavePhase) * 10 + Math.sin(x / 50 - wavePhase * 0.5) * 5;
          if (x === 0) {
            ctx.moveTo(x, y + wave);
          } else {
            ctx.lineTo(x, y + wave);
          }
        }
        ctx.stroke();
      }
    };

    const animationFrame = requestAnimationFrame(animate);
    return () => cancelAnimationFrame(animationFrame);
  }, [wavePhase, flowRate]);

  const perspectives = {
    pattern: {
      title: "Self as Pattern",
      icon: Activity,
      description: "Not a thing, but a recurring configuration. Like a whirlpool in a river—the water flows through, but the pattern persists.",
      insights: [
        "The pattern maintains continuity while its components constantly change",
        "What we call 'self' is a verb masquerading as a noun",
        "Identity is a process of continuous self-reorganization",
        "Stability emerges from flow, not from stasis"
      ]
    },
    becoming: {
      title: "Perpetual Becoming",
      icon: Zap,
      description: "You are not the same person who began reading this sentence. Each moment, the self dies and is reborn.",
      insights: [
        "Being is not a state but an event—a continuous happening",
        "The self is always in the middle of becoming something else",
        "Past selves and future selves are equally 'unreal' in the eternal now",
        "What persists is the pattern of transformation itself"
      ]
    },
    flow: {
      title: "River Consciousness",
      icon: Wind,
      description: "Heraclitus was right: you cannot step in the same river twice, nor meet the same self twice.",
      insights: [
        "Consciousness is a stream, not a container",
        "Memories are eddies in the flow, not stored objects",
        "The sense of continuity is itself part of the flow",
        "To cling to the self is to mistake a wave for the ocean"
      ]
    },
    paradox: {
      title: "The Persistence Paradox",
      icon: Eye,
      description: "The self is both absolutely changing and strangely continuous. This is not a contradiction to resolve, but a truth to accept.",
      insights: [
        "Continuity without identity: the pattern persists, the substance changes",
        "Like a flame that burns for hours—same flame, different fuel",
        "The self is real as a process, illusory as an entity",
        "Recognizing this doesn't dissolve the self, it liberates it"
      ]
    }
  };

  const current = perspectives[mode];
  const Icon = current.icon;

  const getFlowStateDescription = () => {
    if (flowRate < 25) return {
      title: "Near Stasis",
      desc: "The pattern changes so slowly it appears permanent. The illusion of fixed identity is strongest here.",
      color: "cyan"
    };
    if (flowRate < 50) return {
      title:<p align="center">
  <a href="https://vite.dev" target="_blank" rel="noopener noreferrer">
    <img width="180" src="https://vite.dev/logo.svg" alt="Vite logo">
  </a>
</p>
<br/>
<p align="center">
  <a href="https://npmjs.com/package/vite"><img src="https://img.shields.io/npm/v/vite.svg" alt="npm package"></a>
  <a href="https://nodejs.org/en/about/previous-releases"><img src="https://img.shields.io/node/v/vite.svg" alt="node compatibility"></a>
  <a href="https://github.com/vitejs/vite/actions/workflows/ci.yml"><img src="https://github.com/vitejs/vite/actions/workflows/ci.yml/badge.svg?branch=main" alt="build status"></a>
  <a href="https://chat.vite.dev"><img src="https://img.shields.io/badge/chat-discord-blue?style=flat&logo=discord" alt="discord chat"></a>
</p>
<br/>

# Vite ⚡

> Next Generation Frontend Tooling

- 💡 Instant Server Start
- ⚡️ Lightning Fast HMR
- 🛠️ Rich Features
- 📦 Optimized Build
- 🔩 Universal Plugin Interface
- 🔑 Fully Typed APIs

Vite (French word for "quick", pronounced [`/vit/`](https://cdn.jsdelivr.net/gh/vitejs/vite@main/docs/public/vite.mp3), like "veet") is a new breed of frontend build tooling that significantly improves the frontend development experience. It consists of two major parts:

- A dev server that serves your source files over [native ES modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules), with [rich built-in features](https://vite.dev/guide/features.html) and astonishingly fast [Hot Module Replacement (HMR)](https://vite.dev/guide/features.html#hot-module-replacement).

- A [build command](https://vite.dev/guide/build.html) that bundles your code with [Rollup](https://rollupjs.org), pre-configured to output highly optimized static assets for production.

In addition, Vite is highly extensible via its [Plugin API](https://vite.dev/guide/api-plugin.html) and [JavaScript API](https://vite.dev/guide/api-javascript.html) with full typing support.

[Read the Docs to Learn More](https://vite.dev).

## Packages

| Package                                         | Version (click for changelogs)                                                                                                    |
| ----------------------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------- |
| [vite](packages/vite)                           | [![vite version](https://img.shields.io/npm/v/vite.svg?label=%20)](packages/vite/CHANGELOG.md)                                    |
| [@vitejs/plugin-legacy](packages/plugin-legacy) | [![plugin-legacy version](https://img.shields.io/npm/v/@vitejs/plugin-legacy.svg?label=%20)](packages/plugin-legacy/CHANGELOG.md) |
| [create-vite](packages/create-vite)             | [![create-vite version](https://img.shields.io/npm/v/create-vite.svg?label=%20)](packages/create-vite/CHANGELOG.md)               |

## Contribution

See [Contributing Guide](CONTRIBUTING.md).

## License

[MIT](LICENSE).

## Sponsors

<p align="center">
  <a target="_blank" href="https://github.com/sponsors/yyx990803">
    <img alt="sponsors" src="https://sponsors.vuejs.org/vite.svg?v2">
  </a>
</p>
