---
name: Striking Lightning
colors:
  surface: '#121317'
  surface-dim: '#121317'
  surface-bright: '#38393d'
  surface-container-lowest: '#0d0e12'
  surface-container-low: '#1a1b20'
  surface-container: '#1e1f24'
  surface-container-high: '#292a2e'
  surface-container-highest: '#343439'
  on-surface: '#e3e2e7'
  on-surface-variant: '#c7c9ab'
  inverse-surface: '#e3e2e7'
  inverse-on-surface: '#2f3035'
  outline: '#919378'
  outline-variant: '#464832'
  surface-tint: '#c0d100'
  primary: '#ffffff'
  on-primary: '#2e3300'
  primary-container: '#dbee00'
  on-primary-container: '#616a00'
  inverse-primary: '#5b6400'
  secondary: '#c6c5cf'
  on-secondary: '#2f3037'
  secondary-container: '#45464e'
  on-secondary-container: '#b5b4bd'
  tertiary: '#ffffff'
  on-tertiary: '#12353a'
  tertiary-container: '#c5e9ef'
  on-tertiary-container: '#486a6f'
  error: '#ffb4ab'
  on-error: '#690005'
  error-container: '#93000a'
  on-error-container: '#ffdad6'
  primary-fixed: '#dbee00'
  primary-fixed-dim: '#c0d100'
  on-primary-fixed: '#1a1e00'
  on-primary-fixed-variant: '#444b00'
  secondary-fixed: '#e3e1eb'
  secondary-fixed-dim: '#c6c5cf'
  on-secondary-fixed: '#1a1b22'
  on-secondary-fixed-variant: '#45464e'
  tertiary-fixed: '#c5e9ef'
  tertiary-fixed-dim: '#aacdd2'
  on-tertiary-fixed: '#001f24'
  on-tertiary-fixed-variant: '#2a4c51'
  background: '#121317'
  on-background: '#e3e2e7'
  surface-variant: '#343439'
typography:
  display-lg:
    fontFamily: Hanken Grotesk
    fontSize: 56px
    fontWeight: '800'
    lineHeight: '1.1'
    letterSpacing: -0.02em
  headline-lg:
    fontFamily: Hanken Grotesk
    fontSize: 32px
    fontWeight: '700'
    lineHeight: '1.2'
  headline-lg-mobile:
    fontFamily: Hanken Grotesk
    fontSize: 24px
    fontWeight: '700'
    lineHeight: '1.2'
  title-md:
    fontFamily: Hanken Grotesk
    fontSize: 20px
    fontWeight: '600'
    lineHeight: '1.4'
  body-lg:
    fontFamily: Hanken Grotesk
    fontSize: 16px
    fontWeight: '400'
    lineHeight: '1.6'
  body-sm:
    fontFamily: Hanken Grotesk
    fontSize: 14px
    fontWeight: '400'
    lineHeight: '1.5'
  label-md:
    fontFamily: JetBrains Mono
    fontSize: 12px
    fontWeight: '500'
    lineHeight: '1'
    letterSpacing: 0.05em
rounded:
  sm: 0.125rem
  DEFAULT: 0.25rem
  md: 0.375rem
  lg: 0.5rem
  xl: 0.75rem
  full: 9999px
spacing:
  base: 8px
  xs: 4px
  sm: 12px
  md: 24px
  lg: 48px
  xl: 80px
  container-max: 1280px
  gutter: 24px
---

## Brand & Style
The brand personality is high-energy, technical, and ultra-modern. It targets a tech-savvy audience that values speed, precision, and a "developer-centric" aesthetic. The UI should evoke a sense of controlled power—like a high-end performance machine idling in the dark.

The design style is **Dark Glassmorphism mixed with High-Contrast Cyberpunk**. It utilizes deep, saturated blacks as the canvas, punctuated by a singular, piercing neon accent. Elements leverage frosted transparency and "electric" glows to create a sense of depth and luminescence without cluttering the interface with unnecessary textures.

## Colors
The palette is built on a "Total Dark" foundation to maximize the impact of the electric accent.

- **Primary (Electric Neon):** `#EAFF00` is used sparingly for critical actions, status indicators, and active states. It should feel like it is emitting light.
- **Neutral (Deep Black):** `#121317` serves as the primary background color, providing a high-contrast base for the neon yellow.
- **Secondary (Obsidian):** `#2A2B32` is used for elevated surfaces and containers to create subtle separation from the background.
- **Status Colors:** Use de-saturated versions of red and green to ensure they do not compete with the primary neon yellow.

## Typography
The typography system relies on **Hanken Grotesk** for its sharp, contemporary geometry. To maintain readability against the dark background, font weights are slightly heavier than standard light-mode equivalents.

**JetBrains Mono** is introduced as a secondary label font to reinforce the technical, "lightning-fast" nature of the design system. Use it for metadata, code snippets, and small utility labels. Tighten letter spacing on large display text to create a compact, high-impact feel.

## Layout & Spacing
The layout uses a **12-column fluid grid** for desktop, transitioning to a **4-column grid** for mobile. Spacing is strictly based on an 8px scale to maintain mathematical harmony.

- **Margins:** 24px on mobile, scaling to 48px or "auto" on large desktops.
- **Gutters:** Fixed 24px to ensure breathing room between glass modules.
- **Philosophy:** Groups related content within glass containers. Use generous "xl" spacing between major sections to emphasize the void of the deep black background.

## Elevation & Depth
Depth is created through **Glassmorphism and Internal Glows** rather than traditional drop shadows.

1.  **Level 0 (Base):** Solid `#121317`.
2.  **Level 1 (Surface):** Semi-transparent glass with a `12px` backdrop blur and a `1px` stroke at 10% white opacity.
3.  **Level 2 (Active/Floating):** Similar to Level 1, but with a `1px` border using a gradient of white (20%) to the primary neon color (40%). Add a subtle `0px 0px 15px` outer glow using `accent_glow_hex` to simulate electrical discharge.
4.  **Interaction:** On hover, the backdrop blur intensity increases, and the neon border opacity brightens.

## Shapes
The shape language is **Soft yet Precise**. 

We use a 0.25rem (4px) base radius for small components like tags and checkboxes, and 0.75rem (12px) for larger cards and modals. This "Soft" approach prevents the UI from feeling too aggressive while maintaining the technical edge of the Hanken Grotesk typeface. Interactive elements like buttons should never be fully circular unless they are icon-only.

## Components
- **Buttons:** Primary buttons are solid `#EAFF00` with black text. Secondary buttons use the "glass" style with a neon-colored border and no fill.
- **Input Fields:** Use a dark, de-saturated background (`#1C1D22`) with a bottom-only neon border that illuminates (scales width) when focused.
- **Cards:** Utilize the Level 1 elevation (backdrop blur). If a card is "Featured," apply a subtle top-right corner glow in the primary neon yellow.
- **Chips/Tags:** Monospaced labels (JetBrains Mono) inside small, low-opacity glass containers with 4px rounded corners.
- **Progress Bars/Indicators:** Use a "flicker" animation on load to mimic a power-up sequence, utilizing the primary neon color as the fill.
- **Dividers:** Use very thin (1px) lines with a linear gradient that fades into the background at the edges.