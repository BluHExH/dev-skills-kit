---
name: 3d-website
description: Use when building high-end interactive 3D websites, scroll-driven 3D experiences, immersive landing pages, or product showcases with React Three Fiber, Three.js, Spline, or WebGL. Triggers on 3D scenes, scroll animations, 3D models, camera control, performance optimization, and modern immersive web experiences.
---

# 3D Website Development (World-Class Guide)

This skill provides production-grade guidance for building modern interactive 3D websites. Focus is on React Three Fiber (R3F) as the primary stack in 2026, with deep coverage of scroll-driven experiences.

## Recommended Stack (2026)

**Primary Stack (Recommended for most projects):**
- Next.js (App Router) + TypeScript
- React Three Fiber + Three.js
- @react-three/drei (essential helpers)
- @react-three/postprocessing (bloom, depth of field, etc.)
- GSAP + ScrollTrigger (best for complex scroll timelines)
- Tailwind CSS (for HTML/UI overlay)
- Zustand or Jotai (lightweight state if needed)

**When to use alternatives:**
- Spline → Fast prototyping or designer-friendly projects
- Theatre.js → Complex non-scroll timeline animation
- Pure Three.js → Maximum performance or non-React projects
- Babylon.js → Heavy game-like experiences

## Project Setup

```bash
npx create-next-app@latest my-3d-site --typescript --tailwind --eslint --app --src-dir
cd my-3d-site
npm install three @react-three/fiber @react-three/drei @react-three/postprocessing gsap @gsap/react
npm install -D @types/three
```

Recommended folder structure:

```
src/
├── app/
├── components/
│   ├── canvas/           # All R3F components
│   │   ├── Scene.tsx
│   │   ├── Model.tsx
│   │   └── Effects.tsx
│   └── ui/               # Normal HTML/React UI
├── hooks/
├── lib/
└── types/
```

## Core Principles

1. **3D must serve the story** — Never add 3D just because it looks cool.
2. **Performance first** — Especially on mobile.
3. **Progressive enhancement** — Site should still work if WebGL fails.
4. **Accessibility** — Respect `prefers-reduced-motion`.
5. **Separation of concerns** — Keep 3D logic and UI logic clearly separated.

## Basic Canvas Setup

```tsx
"use client"

import { Canvas } from "@react-three/fiber"
import { Suspense } from "react"
import { Preload } from "@react-three/drei"
import Scene from "./Scene"

export default function Experience() {
  return (
    <div className="fixed inset-0 z-0">
      <Canvas
        camera={{ position: [0, 0, 5], fov: 45 }}
        dpr={[1, 1.5]}                    // Cap DPR for performance
        gl={{ antialias: true, alpha: true }}
        shadows
      >
        <Suspense fallback={null}>
          <Scene />
          <Preload all />
        </Suspense>
      </Canvas>
    </div>
  )
}
```

## Scroll-Driven 3D Experiences (Most Requested Pattern)

This is the current industry standard for premium 3D websites (Apple-style product pages, agency sites, etc.).

### Recommended Approach: GSAP ScrollTrigger + R3F

```tsx
"use client"

import { useFrame } from "@react-three/fiber"
import { useRef } from "react"
import gsap from "gsap"
import { ScrollTrigger } from "gsap/ScrollTrigger"
import { useGSAP } from "@gsap/react"

gsap.registerPlugin(ScrollTrigger)

export default function ScrollScene() {
  const group = useRef<THREE.Group>(null)

  useGSAP(() => {
    if (!group.current) return

    gsap.timeline({
      scrollTrigger: {
        trigger: "body",
        start: "top top",
        end: "bottom bottom",
        scrub: 1,               // Smooth scrubbing
      }
    })
    .to(group.current.rotation, { y: Math.PI * 2, ease: "none" })
    .to(group.current.position, { z: -3, ease: "power1.inOut" }, 0)
  }, [])

  return (
    <group ref={group}>
      {/* Your models / meshes */}
    </group>
  )
}
```

### Alternative: drei ScrollControls (Simpler but less flexible)

```tsx
import { ScrollControls, Scroll, useScroll } from "@react-three/drei"

function Scene() {
  return (
    <ScrollControls pages={3} damping={0.2}>
      <Scroll>
        {/* 3D content that moves with scroll */}
      </Scroll>
      <Scroll html>
        {/* HTML content that scrolls normally */}
      </Scroll>
    </ScrollControls>
  )
}
```

**When to choose which:**
- Complex timelines, multiple sections, precise control → **GSAP ScrollTrigger**
- Simpler scroll-linked movement → **drei ScrollControls**

## Model Loading & Optimization

Always optimize models before using them in production.

```tsx
import { useGLTF } from "@react-three/drei"

useGLTF.preload("/models/product.glb")

function Product(props) {
  const { scene } = useGLTF("/models/product.glb")
  return <primitive object={scene} {...props} />
}
```

**Optimization checklist:**
- Use `gltf-transform` or gltf.report
- Prefer GLB over GLTF
- Compress textures (KTX2 / Basis Universal)
- Reduce polygon count aggressively for mobile
- Use Draco compression when beneficial
- Remove unused materials and animations

## Performance Rules (Non-Negotiable)

| Rule | Target |
|------|--------|
| Draw calls | < 80-100 for marketing sites |
| DPR | `[1, 1.5]` or lower on mobile |
| Shadows | Soft shadows only when necessary. Avoid on mobile |
| Lights | Prefer environment maps + few real lights |
| useFrame | Keep extremely light. Avoid heavy calculations |
| Textures | Power-of-two, compressed, properly sized |
| Instancing | Always use for repeated objects |

Use `r3f-perf` during development:

```tsx
import { Perf } from "r3f-perf"
// Inside Canvas
{process.env.NODE_ENV === "development" && <Perf position="top-left" />}
```

## Lighting & Atmosphere

Modern premium look usually comes from:

- Good HDRI environment map (`<Environment preset="city" />` or custom)
- Soft directional light + ambient
- Subtle postprocessing (bloom, vignette, mild chromatic aberration)
- Avoid overusing effects

```tsx
import { EffectComposer, Bloom, Vignette } from "@react-three/postprocessing"

<EffectComposer>
  <Bloom luminanceThreshold={1} intensity={0.4} />
  <Vignette eskil={false} offset={0.1} darkness={0.7} />
</EffectComposer>
```

## Accessibility & Reduced Motion

Always respect user preference:

```tsx
const prefersReducedMotion = window.matchMedia("(prefers-reduced-motion: reduce)").matches

// Disable or simplify heavy animations when true
```

Provide a non-3D fallback version of key content when possible.

## Common Mistakes to Avoid

1. Putting too much logic inside `useFrame`
2. Loading unoptimized 50MB models
3. Using high DPR on mobile
4. Creating new materials/geometries inside render loops
5. Forgetting to dispose resources
6. Making 3D the main content instead of supporting content
7. Ignoring touch devices and performance testing

## Production Checklist

- [ ] Models optimized and compressed
- [ ] DPR capped
- [ ] Loading states + Suspense handled
- [ ] Reduced motion support
- [ ] Tested on mid-range Android
- [ ] Lighthouse performance checked
- [ ] Memory leaks tested (leave page open 5+ minutes)
- [ ] Fallback for WebGL not supported

## Useful Tools

- **gltf.report** / **gltf-transform** → Model optimization
- **Poly Pizza**, **Sketchfab**, **Quaternius** → Free models
- **Spline** → Fast 3D design
- **Leva** → Debug GUI
- **r3f-perf** → Real-time performance
- **Theatre.js** → Advanced animation timeline
- **GSAP ScrollTrigger** → Best scroll control

## When Generating Code

1. Default to React Three Fiber + TypeScript.
2. Prefer GSAP ScrollTrigger for serious scroll experiences.
3. Always include performance considerations.
4. Separate Canvas from HTML UI.
5. Add proper loading and error handling.
6. Suggest model optimization steps.
7. Include reduced-motion support when animations are heavy.
