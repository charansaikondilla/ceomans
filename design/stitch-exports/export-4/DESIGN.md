---
name: Striking Lightning Light
colors:
  surface: '#f8f9fa'
  surface-dim: '#d9dadb'
  surface-bright: '#f8f9fa'
  surface-container-lowest: '#ffffff'
  surface-container-low: '#f3f4f5'
  surface-container: '#edeeef'
  surface-container-high: '#e7e8e9'
  surface-container-highest: '#e1e3e4'
  on-surface: '#191c1d'
  on-surface-variant: '#464832'
  inverse-surface: '#2e3132'
  inverse-on-surface: '#f0f1f2'
  outline: '#77795f'
  outline-variant: '#c7c9ab'
  surface-tint: '#5b6400'
  primary: '#5b6400'
  on-primary: '#ffffff'
  primary-container: '#eaff00'
  on-primary-container: '#6a7400'
  inverse-primary: '#c0d100'
  secondary: '#5e5e5e'
  on-secondary: '#ffffff'
  secondary-container: '#e2e2e2'
  on-secondary-container: '#646464'
  tertiary: '#5c5e61'
  on-tertiary: '#ffffff'
  tertiary-container: '#f2f2f6'
  on-tertiary-container: '#6c6e71'
  error: '#ba1a1a'
  on-error: '#ffffff'
  error-container: '#ffdad6'
  on-error-container: '#93000a'
  primary-fixed: '#dbee00'
  primary-fixed-dim: '#c0d100'
  on-primary-fixed: '#1a1e00'
  on-primary-fixed-variant: '#444b00'
  secondary-fixed: '#e2e2e2'
  secondary-fixed-dim: '#c6c6c6'
  on-secondary-fixed: '#1b1b1b'
  on-secondary-fixed-variant: '#474747'
  tertiary-fixed: '#e1e2e6'
  tertiary-fixed-dim: '#c5c6ca'
  on-tertiary-fixed: '#191c1e'
  on-tertiary-fixed-variant: '#45474a'
  background: '#f8f9fa'
  on-background: '#191c1d'
  surface-variant: '#e1e3e4'
typography:
  headline-xl:
    fontFamily: Hanken Grotesk
    fontSize: 60px
    fontWeight: '800'
    lineHeight: '1.1'
    letterSpacing: -0.04em
  headline-xl-mobile:
    fontFamily: Hanken Grotesk
    fontSize: 40px
    fontWeight: '800'
    lineHeight: '1.1'
    letterSpacing: -0.03em
  headline-lg:
    fontFamily: Hanken Grotesk
    fontSize: 32px
    fontWeight: '700'
    lineHeight: '1.2'
    letterSpacing: -0.02em
  headline-md:
    fontFamily: Hanken Grotesk
    fontSize: 24px
    fontWeight: '700'
    lineHeight: '1.3'
    letterSpacing: -0.01em
  body-lg:
    fontFamily: Hanken Grotesk
    fontSize: 18px
    fontWeight: '400'
    lineHeight: '1.6'
    letterSpacing: '0'
  body-md:
    fontFamily: Hanken Grotesk
    fontSize: 16px
    fontWeight: '400'
    lineHeight: '1.5'
    letterSpacing: '0'
  label-md:
    fontFamily: Hanken Grotesk
    fontSize: 14px
    fontWeight: '600'
    lineHeight: '1'
    letterSpacing: 0.05em
  label-sm:
    fontFamily: Hanken Grotesk
    fontSize: 12px
    fontWeight: '500'
    lineHeight: '1'
    letterSpacing: 0.02em
rounded:
  sm: 0.25rem
  DEFAULT: 0.5rem
  md: 0.75rem
  lg: 1rem
  xl: 1.5rem
  full: 9999px
spacing:
  unit: 4px
  xs: 4px
  sm: 8px
  md: 16px
  lg: 24px
  xl: 40px
  2xl: 64px
  gutter: 24px
  margin-mobile: 16px
  margin-desktop: 48px
---

## Brand & Style

The design system is a premium, high-performance visual framework inspired by high-end technology hardware and precision engineering. It transitions the energy of the original aesthetic into an airy, professional light mode that emphasizes clarity, sophisticated whitespace, and "industrial-clean" aesthetics.

The brand personality is clinical yet energetic—balancing the stability of a professional SaaS platform with the sudden, high-voltage intensity of a precision tool. The target audience includes developers, technical founders, and hardware enthusiasts who value efficiency and high-fidelity interfaces. The emotional response should be one of absolute reliability and forward-thinking innovation.

The design style is **Minimalist-Technic**. It utilizes heavy whitespace, razor-sharp alignment, and a limited but high-impact color palette. It avoids unnecessary decoration, relying instead on high-quality typography and a single "electric" accent to guide the eye.

## Colors

The palette is centered on the contrast between a pure, sterile environment and a high-visibility accent. 

- **Primary (Electric Yellow):** Used sparingly for high-action focal points. Because of its high luminance, it must always be paired with dark text (Secondary) or black iconography to ensure WCAG compliance on light backgrounds.
- **Secondary (Obsidian):** Used for primary text, deep borders, and structural elements to provide "weight" to the airy layout.
- **Neutral/Surface:** A range of clean whites and cool-toned off-whites (#F8F9FA) create a layered, "hardware" feel, separating content modules without the need for heavy shadows.
- **Status:** Use pure, high-saturation tones for functional feedback (Success: #00C853, Error: #FF1744), ensuring they match the intensity of the primary yellow.

## Typography

The typography system utilizes **Hanken Grotesk** exclusively to maintain a sharp, contemporary, and engineered feel. 

Large displays and headlines use heavy weights (700-800) with tight letter-spacing to mimic technical documentation and high-end branding. Body text is kept clean and legible with generous line heights to ensure the interface feels "airy."

Label styles are frequently used for metadata and technical indicators, often employing uppercase transformations and increased letter spacing to differentiate them from prose.

## Layout & Spacing

The layout follows a strict **Fluid Grid** model based on a 4px baseline unit. 

- **Desktop:** 12-column grid with 24px gutters. Content is often center-aligned with wide margins to emphasize a premium, editorial feel.
- **Tablet:** 8-column grid with 20px gutters.
- **Mobile:** 4-column grid with 16px gutters and 16px side margins.

The spacing rhythm prioritizes "clumping" related information tightly while using large `2xl` gaps to separate major sections, creating a clear visual hierarchy through negative space rather than lines.

## Elevation & Depth

This design system eschews traditional soft shadows in favor of **Tonal Layers** and **Low-Contrast Outlines**.

Depth is communicated through subtle shifts in surface color (e.g., a white #FFFFFF card on a #F8F9FA background). To define interactive elements, use 1px solid borders in Obsidian (#000000) or Cool Gray (#E2E4E9). 

When elevation is absolutely necessary (e.g., floating menus), use a "Technical Shadow": a zero-blur, offset shadow (e.g., 4px 4px 0px) in a light gray or the primary accent color to maintain the crisp, graphic nature of the system.

## Shapes

The shape language is defined by **Rounded (Level 2)** corners. This 0.5rem (8px) base radius softens the "industrial" sharpness just enough to make the interface feel modern and approachable without becoming "bubbly" or playful.

- Small components (Inputs, Buttons): 8px (0.5rem)
- Large components (Cards, Containers): 16px (1rem)
- Feature elements (Hero images): 24px (1.5rem)

## Components

- **Buttons:** Primary buttons use the Electric Yellow background with black text. Secondary buttons use a transparent background with a 1.5px Obsidian border. All buttons should have a subtle 1px top-light inner highlight to feel tactile.
- **Inputs:** Use a #F8F9FA background with an Obsidian bottom-border only for a "technical form" look, or a full thin border. Focus states must use the Electric Yellow as a 2px outer ring.
- **Cards:** White (#FFFFFF) background with a very thin (#E2E4E9) border. No shadow. The header of the card may use a small vertical bar of Electric Yellow to denote importance.
- **Chips:** Small, pill-shaped elements with #F8F9FA backgrounds and Secondary (#000000) text.
- **Lists:** Separated by thin, subtle horizontal rules (#E2E4E9). Hover states use a very pale tint of the primary color or a simple 4px shift to the right.
- **Data Tables:** High-density, utilizing the label-sm typography for headers. Alternate row striping is discouraged; use subtle border-bottoms instead.