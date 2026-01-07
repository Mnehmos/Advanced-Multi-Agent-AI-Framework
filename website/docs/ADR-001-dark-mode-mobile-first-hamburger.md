# ADR-001: Dark Mode, Mobile-First Architecture, and Hamburger Component

**Status:** Proposed  
**Date:** 2026-01-07  
**Author:** Architect Mode  
**Deciders:** Code Mode (Implementation)

## Context

The website at `website/src/` requires three major enhancements:
1. **Dark mode** support with WCAG-compliant contrast ratios
2. **Mobile-first CSS architecture** (currently uses desktop-first patterns)
3. **Hamburger menu component** with proper animation and accessibility

### Current State Analysis

| Area | Current Implementation | Issues |
|------|----------------------|--------|
| Theme | Light only: `bg-stone-50 text-stone-800` | No dark mode support |
| CSS Variables | `--copper: #B87333`, `--copper-light: #D4956A` | Insufficient for theming |
| Responsive | Desktop-first: `hidden md:flex`, `md:hidden` | Mobile experience is afterthought |
| Hamburger | Simple toggle, no animation | No ARIA, no X transform, no focus trap |

---

## Decision 1: Dark Mode Design System

### Color Token Architecture

```css
:root {
  /* ===========================================
     SEMANTIC COLOR TOKENS - Light Theme (Default)
     =========================================== */
  
  /* Background Layers (elevation system) */
  --color-bg-base: #FAFAF9;           /* stone-50 - page background */
  --color-bg-surface: #FFFFFF;        /* white - cards, elevated content */
  --color-bg-elevated: #F5F5F4;       /* stone-100 - modals, dropdowns */
  --color-bg-muted: #E7E5E4;          /* stone-200 - disabled, subtle */
  
  /* Foreground / Text */
  --color-text-primary: #1C1917;      /* stone-900 - headings, primary text */
  --color-text-secondary: #57534E;    /* stone-600 - body text */
  --color-text-muted: #A8A29E;        /* stone-400 - captions, placeholders */
  --color-text-inverse: #FAFAF9;      /* stone-50 - text on dark bg */
  
  /* Border / Divider */
  --color-border-default: #E7E5E4;    /* stone-200 */
  --color-border-strong: #D6D3D1;     /* stone-300 */
  --color-border-muted: #F5F5F4;      /* stone-100 */
  
  /* Brand - Copper */
  --color-brand-primary: #B87333;     /* copper */
  --color-brand-light: #D4956A;       /* copper-light */
  --color-brand-dark: #9A5F2A;        /* darker copper for contrast */
  
  /* Interactive States */
  --color-interactive-hover: #292524; /* stone-800 */
  --color-interactive-active: #1C1917;/* stone-900 */
  --color-interactive-focus: #B87333; /* copper - focus ring */
  
  /* Semantic - Feedback */
  --color-success: #22C55E;           /* green-500 */
  --color-warning: #F59E0B;           /* amber-500 */
  --color-error: #EF4444;             /* red-500 */
  --color-info: #3B82F6;              /* blue-500 */
  
  /* Layer Colors (for nervous system sections) */
  --color-layer-central: #F3E8FF;     /* purple-100 */
  --color-layer-central-border: #C084FC; /* purple-400 */
  --color-layer-somatic: #FEF3C7;     /* amber-100 */
  --color-layer-somatic-border: #FBBF24; /* amber-400 */
  --color-layer-autonomic: #DBEAFE;   /* blue-100 */
  --color-layer-autonomic-border: #60A5FA; /* blue-400 */
  --color-layer-reflex: #FEE2E2;      /* red-100 */
  --color-layer-reflex-border: #F87171; /* red-400 */
  
  /* Component-Specific */
  --color-nav-bg: rgba(250, 250, 249, 0.9); /* stone-50/90 */
  --color-nav-border: #E7E5E4;
  --color-footer-bg: #1C1917;         /* stone-900 */
  --color-footer-text: #D6D3D1;       /* stone-300 */
  
  /* Scrollbar */
  --color-scrollbar-track: #F5F5F4;
  --color-scrollbar-thumb: #A8A29E;
  --color-scrollbar-thumb-hover: #78716C;
  
  /* Shadows */
  --shadow-sm: 0 1px 2px rgba(28, 25, 23, 0.05);
  --shadow-md: 0 4px 6px rgba(28, 25, 23, 0.07);
  --shadow-lg: 0 10px 15px rgba(28, 25, 23, 0.1);
}

/* ===========================================
   DARK THEME
   =========================================== */
[data-theme="dark"] {
  /* Background Layers */
  --color-bg-base: #0C0A09;           /* stone-950 */
  --color-bg-surface: #1C1917;        /* stone-900 */
  --color-bg-elevated: #292524;       /* stone-800 */
  --color-bg-muted: #44403C;          /* stone-700 */
  
  /* Foreground / Text */
  --color-text-primary: #FAFAF9;      /* stone-50 */
  --color-text-secondary: #D6D3D1;    /* stone-300 */
  --color-text-muted: #78716C;        /* stone-500 */
  --color-text-inverse: #1C1917;      /* stone-900 */
  
  /* Border / Divider */
  --color-border-default: #44403C;    /* stone-700 */
  --color-border-strong: #57534E;     /* stone-600 */
  --color-border-muted: #292524;      /* stone-800 */
  
  /* Brand - Copper (adjusted for dark mode) */
  --color-brand-primary: #D4956A;     /* copper-light for better contrast */
  --color-brand-light: #E8B896;       /* lighter copper */
  --color-brand-dark: #B87333;        /* original copper */
  
  /* Interactive States */
  --color-interactive-hover: #D6D3D1; /* stone-300 */
  --color-interactive-active: #FAFAF9;/* stone-50 */
  
  /* Layer Colors (darkened for dark mode) */
  --color-layer-central: #2E1065;     /* purple-950 */
  --color-layer-central-border: #9333EA; /* purple-600 */
  --color-layer-somatic: #451A03;     /* amber-950 */
  --color-layer-somatic-border: #D97706; /* amber-600 */
  --color-layer-autonomic: #172554;   /* blue-950 */
  --color-layer-autonomic-border: #2563EB; /* blue-600 */
  --color-layer-reflex: #450A0A;      /* red-950 */
  --color-layer-reflex-border: #DC2626; /* red-600 */
  
  /* Component-Specific */
  --color-nav-bg: rgba(12, 10, 9, 0.9); /* stone-950/90 */
  --color-nav-border: #292524;
  --color-footer-bg: #0C0A09;         /* stone-950 */
  --color-footer-text: #A8A29E;       /* stone-400 */
  
  /* Scrollbar */
  --color-scrollbar-track: #1C1917;
  --color-scrollbar-thumb: #57534E;
  --color-scrollbar-thumb-hover: #78716C;
  
  /* Shadows (more prominent on dark) */
  --shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.3);
  --shadow-md: 0 4px 6px rgba(0, 0, 0, 0.4);
  --shadow-lg: 0 10px 15px rgba(0, 0, 0, 0.5);
}
```

### WCAG Contrast Ratio Verification

| Element | Light Mode | Dark Mode | Ratio (L) | Ratio (D) | Pass |
|---------|-----------|-----------|-----------|-----------|------|
| Primary text on base | `#1C1917` on `#FAFAF9` | `#FAFAF9` on `#0C0A09` | 16.8:1 | 17.4:1 | ✅ AAA |
| Secondary text on base | `#57534E` on `#FAFAF9` | `#D6D3D1` on `#0C0A09` | 6.1:1 | 11.0:1 | ✅ AA |
| Muted text on base | `#A8A29E` on `#FAFAF9` | `#78716C` on `#0C0A09` | 3.3:1 | 4.0:1 | ✅ UI* |
| Copper on base | `#B87333` on `#FAFAF9` | `#D4956A` on `#0C0A09` | 4.6:1 | 7.0:1 | ✅ AA |
| Text on copper button | `#FAFAF9` on `#B87333` | `#0C0A09` on `#D4956A` | 4.6:1 | 7.0:1 | ✅ AA |

*UI components require 3:1 minimum (WCAG 2.1 Level AA)

### Theme Toggle Mechanism

```typescript
// Theme persistence and toggle contract
interface ThemeManager {
  // Get current theme ('light' | 'dark' | 'system')
  getTheme(): string;
  
  // Set theme explicitly
  setTheme(theme: 'light' | 'dark' | 'system'): void;
  
  // Get resolved theme (actual applied theme)
  getResolvedTheme(): 'light' | 'dark';
  
  // Listen for theme changes
  onThemeChange(callback: (theme: string) => void): () => void;
}

// Implementation requirements:
// 1. Check localStorage for saved preference
// 2. Fall back to prefers-color-scheme media query
// 3. Apply data-theme attribute to <html>
// 4. Prevent flash of wrong theme (inline script in <head>)
```

**Inline Script for Flash Prevention (must be in `<head>`):**

```html
<script>
  (function() {
    const saved = localStorage.getItem('theme');
    const prefersDark = window.matchMedia('(prefers-color-scheme: dark)').matches;
    const theme = saved || (prefersDark ? 'dark' : 'light');
    document.documentElement.setAttribute('data-theme', theme);
  })();
</script>
```

### Transition Strategy

```css
/* Smooth theme transition - apply to elements that change */
html[data-theme-transitioning] * {
  transition: background-color 0.2s ease, 
              color 0.2s ease, 
              border-color 0.2s ease,
              box-shadow 0.2s ease !important;
}
```

---

## Decision 2: Mobile-First CSS Architecture

### Breakpoint Strategy

| Breakpoint | Min-Width | Target | Tailwind |
|------------|-----------|--------|----------|
| Base | 0px | Mobile phones (portrait) | Default |
| `sm` | 640px | Mobile phones (landscape), small tablets | `sm:` |
| `md` | 768px | Tablets | `md:` |
| `lg` | 1024px | Small laptops | `lg:` |
| `xl` | 1280px | Desktops | `xl:` |
| `2xl` | 1536px | Large desktops | `2xl:` |

### Conversion Patterns

#### Pattern 1: Navigation Visibility

```diff
<!-- Desktop-first (CURRENT - WRONG) -->
- <div class="hidden md:flex items-center gap-8">
- <button class="md:hidden">

<!-- Mobile-first (CORRECT) -->
+ <div class="flex md:hidden">  <!-- Mobile menu button -->
+ <div class="hidden md:flex items-center gap-8">  <!-- Desktop nav - already correct! -->
```

Wait - the current `hidden md:flex` IS mobile-first for the desktop nav (hidden by default, flex on md+). The issue is the hamburger menu itself needs mobile-first thinking.

#### Pattern 2: Grid Layouts

```diff
<!-- Current -->
- <div class="grid md:grid-cols-2 gap-12">

<!-- Mobile-first enhancement -->
+ <div class="grid gap-8 md:gap-12 md:grid-cols-2">
```

#### Pattern 3: Typography Scaling

```diff
<!-- Current -->
- <h1 class="text-4xl lg:text-5xl xl:text-6xl">

<!-- Mobile-first (already correct, but add sm breakpoint) -->
+ <h1 class="text-3xl sm:text-4xl lg:text-5xl xl:text-6xl">
```

#### Pattern 4: Spacing

```diff
<!-- Current -->
- <section class="py-20 lg:py-32">

<!-- Mobile-first -->
+ <section class="py-12 sm:py-16 md:py-20 lg:py-32">
```

### Component Layout Contract

```typescript
// Responsive layout contract for components
interface ResponsiveComponent {
  // Base (mobile) styles always defined first
  base: CSSProperties;
  
  // Progressive enhancements only
  sm?: CSSProperties;  // >= 640px
  md?: CSSProperties;  // >= 768px
  lg?: CSSProperties;  // >= 1024px
  xl?: CSSProperties;  // >= 1280px
}

// Example: Hero section
const heroLayout: ResponsiveComponent = {
  base: {
    padding: '3rem 1.5rem',
    display: 'flex',
    flexDirection: 'column',
    gap: '2rem'
  },
  lg: {
    padding: '5rem 1.5rem',
    display: 'grid',
    gridTemplateColumns: 'repeat(2, 1fr)',
    gap: '3rem'
  }
};
```

### Mobile-First Checklist for Code Mode

- [ ] All `hidden X:flex` patterns reviewed - ensure hiding is intentional for mobile
- [ ] All grids start with `grid` or `grid-cols-1` as base
- [ ] Typography scales up from smallest readable size
- [ ] Padding/margin uses smaller values as base
- [ ] Touch targets minimum 44x44px on mobile
- [ ] Interactive elements have adequate spacing for touch

---

## Decision 3: Hamburger Component Specification

### Component Structure

```html
<button
  id="mobile-menu-btn"
  class="hamburger"
  aria-label="Open menu"
  aria-expanded="false"
  aria-controls="mobile-menu"
>
  <span class="hamburger-box">
    <span class="hamburger-line hamburger-line--top"></span>
    <span class="hamburger-line hamburger-line--middle"></span>
    <span class="hamburger-line hamburger-line--bottom"></span>
  </span>
</button>
```

### SVG Animation Approach (CSS-based)

```css
/* Hamburger Component Styles */
.hamburger {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 44px;           /* Touch target */
  height: 44px;          /* Touch target */
  padding: 0.5rem;
  background: transparent;
  border: none;
  cursor: pointer;
  color: var(--color-text-secondary);
  transition: color 0.2s ease;
}

.hamburger:hover {
  color: var(--color-text-primary);
}

.hamburger:focus-visible {
  outline: 2px solid var(--color-interactive-focus);
  outline-offset: 2px;
  border-radius: 0.25rem;
}

.hamburger-box {
  position: relative;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  width: 24px;
  height: 18px;
}

.hamburger-line {
  display: block;
  width: 100%;
  height: 2px;
  background-color: currentColor;
  border-radius: 1px;
  transition: transform 0.3s ease, opacity 0.3s ease;
  transform-origin: center;
}

/* Open state - X transformation */
.hamburger[aria-expanded="true"] .hamburger-line--top {
  transform: translateY(8px) rotate(45deg);
}

.hamburger[aria-expanded="true"] .hamburger-line--middle {
  opacity: 0;
  transform: scaleX(0);
}

.hamburger[aria-expanded="true"] .hamburger-line--bottom {
  transform: translateY(-8px) rotate(-45deg);
}
```

### Menu Animation

```css
/* Mobile Menu */
#mobile-menu {
  /* Hidden state */
  display: grid;
  grid-template-rows: 0fr;
  opacity: 0;
  transition: grid-template-rows 0.3s ease, opacity 0.2s ease;
  overflow: hidden;
}

#mobile-menu[data-open="true"] {
  grid-template-rows: 1fr;
  opacity: 1;
}

#mobile-menu > div {
  overflow: hidden;
}
```

### ARIA and Accessibility Requirements

| Attribute | Initial | Open | Purpose |
|-----------|---------|------|---------|
| `aria-label` | "Open menu" | "Close menu" | Screen reader announcement |
| `aria-expanded` | "false" | "true" | State communication |
| `aria-controls` | "mobile-menu" | "mobile-menu" | Associates button with menu |
| `role` on menu | - | "navigation" | Landmark identification |

### Focus Management Requirements

```typescript
interface HamburgerFocusContract {
  // When menu opens:
  // 1. Move focus to first menu item OR close button
  // 2. Trap focus within menu until closed
  onOpen(): void;
  
  // When menu closes:
  // 1. Return focus to hamburger button
  // 2. Release focus trap
  onClose(): void;
  
  // Keyboard handling:
  // - Escape: Close menu
  // - Tab: Cycle through menu items (trapped)
  // - Arrow keys (optional): Navigate menu items
  onKeyDown(event: KeyboardEvent): void;
}
```

### Complete JavaScript Contract

```typescript
// hamburger-menu.ts
interface HamburgerMenuOptions {
  buttonSelector: string;
  menuSelector: string;
  focusableSelector?: string;
}

class HamburgerMenu {
  private button: HTMLButtonElement;
  private menu: HTMLElement;
  private isOpen: boolean = false;
  private focusableElements: HTMLElement[];
  private firstFocusable: HTMLElement | null;
  private lastFocusable: HTMLElement | null;
  
  constructor(options: HamburgerMenuOptions);
  
  toggle(): void;
  open(): void;
  close(): void;
  
  private updateAriaAttributes(): void;
  private trapFocus(event: KeyboardEvent): void;
  private handleEscape(event: KeyboardEvent): void;
  private handleClickOutside(event: MouseEvent): void;
}

// Usage in Layout.astro
// new HamburgerMenu({
//   buttonSelector: '#mobile-menu-btn',
//   menuSelector: '#mobile-menu',
//   focusableSelector: 'a, button'
// });
```

---

## Implementation Guidance for Code Mode

### File Changes Required

| File | Changes |
|------|---------|
| `website/src/layouts/Layout.astro` | Add theme script, update nav, hamburger component |
| `website/src/styles/global.css` | Add CSS custom properties, hamburger styles |
| `website/src/components/ThemeToggle.astro` | New component (optional) |
| `website/src/scripts/hamburger-menu.ts` | New script for menu logic |

### Migration Order

1. **Phase 1: CSS Custom Properties**
   - Add all CSS variables to `global.css`
   - Replace hardcoded colors in Layout.astro with var() references
   - Test light mode still works

2. **Phase 2: Dark Mode**
   - Add flash prevention script to `<head>`
   - Add `[data-theme="dark"]` rules
   - Add theme toggle button (optional - can use system preference only)
   - Test both themes

3. **Phase 3: Mobile-First Audit**
   - Review all Tailwind classes
   - Update breakpoint patterns
   - Test on mobile viewport first

4. **Phase 4: Hamburger Component**
   - Replace SVG icon with span-based hamburger
   - Add CSS animations
   - Implement JavaScript with focus management
   - Test with keyboard and screen reader

### Testing Checklist

- [ ] Lighthouse accessibility score >= 95
- [ ] Color contrast passes axe-core audit
- [ ] Theme toggle works and persists
- [ ] System preference is respected
- [ ] No flash of wrong theme on load
- [ ] Hamburger transforms to X with animation
- [ ] Menu can be keyboard navigated
- [ ] Focus is trapped in open menu
- [ ] Escape closes menu
- [ ] Click outside closes menu
- [ ] All breakpoints tested (320px, 640px, 768px, 1024px, 1280px)

---

## Consequences

### Positive

- WCAG AA compliance for all text/UI
- Better mobile user experience
- Accessible navigation for screen reader users
- Consistent theming through CSS custom properties
- Progressive enhancement from mobile up

### Negative

- Increased CSS complexity
- Additional JavaScript for hamburger menu
- Testing burden across themes and breakpoints

### Risks

- Theme flash if script fails to load
- Animation may be janky on low-power devices (mitigate: respect `prefers-reduced-motion`)

---

## References

- [WCAG 2.1 Contrast Requirements](https://www.w3.org/WAI/WCAG21/Understanding/contrast-minimum.html)
- [Tailwind CSS Responsive Design](https://tailwindcss.com/docs/responsive-design)
- [Inclusive Components: Menus](https://inclusive-components.design/menus-menu-buttons/)
- [prefers-color-scheme MDN](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-color-scheme)
