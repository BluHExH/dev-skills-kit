---
name: 3d-website
description: Use when building interactive 3D websites, scroll-driven 3D experiences, product showcases, or immersive landing pages with React Three Fiber, Three.js, or similar. Triggers on 3D scenes, camera control, scroll animations, model loading, performance optimization, lighting, and production 3D web experiences.
---

# 3D Website Development

Production-focused guide for building high-quality interactive 3D websites in 2026. Primary stack is React Three Fiber (R3F) + Three.js.

## Core Philosophy

1. **3D must serve the story** — Never add 3D just for visual flex.
2. **Performance is non-negotiable** — Especially on mobile.
3. **Progressive enhancement** — The site should still function if WebGL fails.
4. **Accessibility matters** — Respect `prefers-reduced-motion`.
5. **Separate concerns** — Keep 3D scene logic and HTML/UI logic clearly separated.

## Recommended Stack (2026)

**Primary:**
- Next.js (App Router) + TypeScript
- React Three Fiber + Three.js
- @react-three/drei (helpers)
- @react-three/postprocessing
- GSAP + ScrollTrigger (for complex scroll timelines)
- Tailwind CSS (UI overlay)

**Alternatives:**
- Spline → Fast prototyping / designer handoff
- Theatre.js → Complex non-scroll timelines
- Pure Three.js → Maximum control / non-React projects

## Project Setup

```bash
npx create-next-app@latest my-3d-site --typescript --tailwind --eslint --app --src-dir
cd my-3d-site
npm install three @react-three/fiber @react-three/drei @react-three/postprocessing gsap @gsap/react
npm install -D @types/three
```

**Recommended structure:**
```
src/
├── app/
├── components/
│   ├── canvas/          # All R3F / 3D components
│   │   ├── Scene.tsx
│   │   ├── Model.tsx
│   │   └── Effects.tsx
│   └── ui/              # Normal HTML UI
├── hooks/
└── lib/
```

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
        dpr={[1, 1.5]}
        gl={{ antialias: true, alpha: true }}
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

## Scroll-Driven 3D (Most Common Premium Pattern)

### Preferred: GSAP ScrollTrigger + R3F

```tsx
"use client"

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
        scrub: 1,
      }
    })
    .to(group.current.rotation, { y: Math.PI * 2, ease: "none" })
    .to(group.current.position, { z: -3, ease: "power1.inOut" }, 0)
  }, [])

  return <group ref={group}>{/* content */}</group>
}
```

### Alternative: drei ScrollControls

Simpler but less flexible for complex timelines.

**When to choose:**
- Complex multi-section cinematic → GSAP ScrollTrigger
- Simple linked movement → drei ScrollControls

## Model Loading & Optimization

```tsx
import { useGLTF } from "@react-three/drei"

useGLTF.preload("/models/product.glb")

function Model(props) {
  const { scene } = useGLTF("/models/product.glb")
  return <primitive object={scene} {...props} />
}
```

**Optimization rules:**
- Always optimize models before production (gltf-transform / gltf.report)
- Prefer GLB
- Compress textures (KTX2 / Basis)
- Reduce poly count aggressively for mobile
- Remove unused materials and animations
- Use Draco when beneficial

## Performance Rules (Critical)

| Area | Target / Rule |
|------|---------------|
| Draw calls | < 80–100 for marketing sites |
| DPR | `[1, 1.5]` or lower on mobile |
| Shadows | Soft only when needed. Avoid on mobile |
| Lights | Prefer environment maps + few real lights |
| useFrame | Keep extremely light |
| Textures | Power-of-two + compressed |
| Repeated objects | Always use instancing |

Use `r3f-perf` in development.

## Lighting & Atmosphere

Modern premium look usually comes from:
- Good HDRI / Environment map
- Soft directional + ambient
- Subtle postprocessing (bloom, vignette)
- Avoid overusing effects

```tsx
import { EffectComposer, Bloom, Vignette } from "@react-three/postprocessing"

<EffectComposer>
  <Bloom luminanceThreshold={1} intensity={0.35} />
  <Vignette offset={0.1} darkness={0.6} />
</EffectComposer>
```

## Accessibility

Always check `prefers-reduced-motion` and reduce or disable heavy animation when true. Provide meaningful fallback content when possible.

## Common Mistakes

1. Heavy logic inside `useFrame`
2. Unoptimized large models
3. High DPR on mobile
4. Creating geometries/materials inside loops
5. Forgetting to dispose resources
6. Making 3D the main content instead of supporting content
7. No testing on real mid-range phones

## Production Checklist

- [ ] Models optimized and compressed
- [ ] DPR capped
- [ ] Loading + Suspense handled
- [ ] Reduced motion support
- [ ] Tested on mid-range Android
- [ ] Memory leaks checked
- [ ] WebGL fallback considered
- [ ] Lighthouse / real device performance reviewed

## Useful Tools

- gltf.report / gltf-transform
- Poly Pizza, Sketchfab, Quaternius
- Spline
- Leva (debug)
- r3f-perf
- Theatre.js
- GSAP ScrollTrigger

## When Generating Code

1. Default to React Three Fiber + TypeScript
2. Prefer GSAP ScrollTrigger for serious scroll experiences
3. Always consider mobile performance
4. Separate Canvas from HTML UI
5. Include loading and error handling
6. Suggest model optimization
7. Support reduced motion when animations are heavy
