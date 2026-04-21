# Image Editor - Specification

## 1. Project Overview

- **Project Name**: Image Editor
- **Type**: Django Web Application
- **Core Functionality**: Real-time image processing with convolution-based filters and pixel manipulation

---

## 2. UI/UX Specification

### Layout Structure

- **Header**: Logo + app name on left, theme toggle on right
- **Main Content**: Two-column layout
  - Left: Large image canvas with floating toolbar overlay
  - Right: Collapsible control panel with sections

### Responsive Breakpoints
- Desktop: Two-column layout
- Tablet/Mobile (<900px): Stacked single column with bottom sheet controls

### Visual Design - Dark Mode (Default)

**Color Palette**:
- Background: `#0d0d0d` (near black)
- Surface: `#1a1a1a` (card background)
- Surface Elevated: `#252525` (hover states)
- Primary: `#6366f1` (indigo)
- Primary Glow: `#818cf8`
- Accent: `#22d3ee` (cyan)
- Text Primary: `#ffffff`
- Text Secondary: `#a1a1aa`
- Border: `#3f3f46`

**Dark Mode Gradients**:
- Header: `linear-gradient(135deg, #1a1a1a 0%, #0d0d0d 100%)`
- Primary Button: `linear-gradient(135deg, #6366f1, #8b5cf6)`
- Accent Glow: `radial-gradient(circle, rgba(99,102,241,0.2) 0%, transparent 70%)`

### Visual Design - Light Mode

**Color Palette**:
- Background: `#f5f5f7`
- Surface: `#ffffff`
- Surface Elevated: `#f0f0f2`
- Primary: `#6366f1`
- Primary Glow: `#a5b4fc`
- Accent: `#0891b2`
- Text Primary: `#18181b`
- Text Secondary: `#71717a`
- Border: `#e4e4e7`

**Light Mode Effects**:
- Card shadow: `0 4px 24px rgba(0,0,0,0.08)`
- Subtle gradients on interactive elements

### Typography

- **Logo Font**: `"Space Grotesk", sans-serif` - bold, 24px
- **Headings**: `"Inter", system-ui` - weight 600
- **Body**: `"Inter", system-ui` - weight 400
- **Mono/Values**: `"JetBrains Mono", monospace` - weight 500

### Logo Design

- **Concept**: Geometric diamond/prism shape with gradient representing pixel manipulation
- **SVG**: Abstract forge/flame icon with pixel grid pattern
- **Colors**: Gradient from `#6366f1` (indigo) to `#22d3ee` (cyan)

### Interactive Elements

**Animations**:
- Theme toggle: 300ms smooth transition with rotation
- Slider interaction: Glow effect on focus, value tooltip appears
- Buttons: Scale 1.02 on hover, subtle shadow lift
- Control sections: Accordion collapse with 200ms ease
- Canvas: Subtle vignette effect, scanline overlay option

**Visual Effects**:
- Glass morphism on control panel (backdrop-filter: blur)
- Gradient borders on active elements
- Animated gradient on primary actions
- Subtle particle/noise texture on surfaces

### Component States

**Buttons**:
- Default: Solid gradient or outlined
- Hover: Scale 1.02, enhanced shadow, glow
- Active: Scale 0.98, brighter glow
- Disabled: 50% opacity, no interactions

**Sliders**:
- Track: Rounded capsule shape
- Fill: Gradient from left to thumb position
- Thumb: Circular with glow ring on hover/active
- Value: Appears above thumb on interaction

**Toggle Switch**:
- iOS-style toggle for theme switch
- Smooth sliding animation
- Icon changes (sun/moon)

---

## 3. Features

All features from original spec plus:
- Theme toggle (persisted to localStorage)
- Animated UI transitions
- Enhanced visual feedback

---

## 4. Acceptance Criteria

- [ ] Cool logo displayed in header
- [ ] Theme toggle switches between light/dark modes
- [ ] Theme preference persisted
- [ ] All controls work properly
- [ ] UI feels modern and interactive
