---
name: 3d-website
description: Use when building interactive 3D websites, landing pages, or experiences with Three.js, React Three Fiber, Spline, or WebGL. Triggers on 3D scenes, 3D models, animations, scroll-based 3D, performance optimization for 3D, and modern 3D web experiences.
---

# 3D Website Development

## Recommended Stack (2026)

**Primary (most recommended):**
- React Three Fiber (R3F) + Three.js
- @react-three/drei (helpers)
- @react-three/postprocessing (effects)
- GSAP or Framer Motion for timeline/scroll animations
- Tailwind CSS for UI overlay

**Alternatives:**
- Spline (fast prototyping, less code)
- Theatre.js (advanced animation timeline)
- Babylon.js (heavier games/experiences)
- Pure Three.js (when maximum control is needed)

## Project Setup (R3F)

```bash
npx create-next-app@latest my-3d-site --typescript --tailwind --eslint --app
cd my-3d-site
npm install three @react-three/fiber @react-three/drei @react-three/postprocessing gsap
```

Basic Canvas structure:

```tsx
"use client"

import { Canvas } from "@react-three/fiber"
import { OrbitControls, Environment, Float } from "@react-three/drei"

export default function Scene() {
  return (
    <Canvas camera={{ position: [0, 0, 5], fov: 45 }}>
      <ambientLight intensity={0.5} />
      <directionalLight position={[10, 10, 5]} intensity={1} />
      <Float>
        {/* Your 3D content */}
      </Float>
      <OrbitControls enableZoom={false} />
      <Environment preset="city" />
    </Canvas>
  )
}
```

## Core Best Practices

- Always use `"use client"` for components that use R3F.
- Keep the Canvas as high as possible in the tree. Avoid putting heavy UI inside the Canvas.
- Prefer declarative components from `@react-three/drei` over raw Three.js when possible.
- Use `useFrame` sparingly and keep it lightweight.
- Dispose geometries, materials, and textures properly to avoid memory leaks.
- Prefer GLTF / GLB models optimized with gltfjsx or gltf-transform.

## Performance Rules (Critical)

1. Keep draw calls low (ideally under 100 for marketing sites).
2. Use instancing for repeated objects.
3. Compress textures (KTX2 / Basis) and models.
4. Use `dpr={[1, 2]}` or lower on mobile.
5. Implement Level of Detail (LOD) for complex models.
6. Avoid real-time shadows on mobile unless necessary.
7. Use `Suspense` + progressive loading for heavy assets.
8. Monitor FPS and memory with `r3f-perf` during development.

## Common Patterns

**Scroll-based 3D (very popular):**
- Use `@react-three/drei` ScrollControls or GSAP ScrollTrigger + useFrame.
- Animate camera or object positions based on scroll progress.

**Hero Section 3D:**
- Floating objects + subtle auto-rotation.
- Mouse parallax with `useThree` and pointer events.

**Model Loading:**
```tsx
import { useGLTF } from "@react-three/drei"

function Model(props) {
  const { scene } = useGLTF("/model.glb")
  return <primitive object={scene} {...props} />
}
```

**Interactive Elements:**
- Use `onClick`, `onPointerOver` on meshes.
- Combine with HTML overlays using `@react-three/drei` Html component.

## Design Guidelines

- 3D should enhance the story, not distract.
- Keep camera movement smooth and purposeful.
- Use lighting and environment maps to create mood.
- Provide reduced-motion alternatives for accessibility.
- Always test on mid-range mobile devices.

## When Generating Code

1. Prefer React Three Fiber over vanilla Three.js unless asked otherwise.
2. Include proper TypeScript types.
3. Add loading states and error boundaries.
4. Consider mobile performance from the start.
5. Separate 3D scene logic from UI logic.
6. Suggest optimization steps when models or effects are complex.

## Useful Tools

- gltf.report / gltf-transform → model optimization
- Poly Pizza / Sketchfab → free models
- Spline → rapid 3D design
- Leva → debug GUI
- r3f-perf → performance monitoring
