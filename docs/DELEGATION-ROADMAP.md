# Delegation Roadmap
## Sub-Agent Task Assignments

**Project:** Meal Planning PWA
**Total Remaining Work:** ~14 features (polish phase)

---

## Phase 1: PWA Enhancement

### Assignment 1.1: Service Worker Implementation
**Delegate To:** @pwa-specialist
**Priority:** CRITICAL
**Branch:** `feature/pwa-offline`

#### Context Package

```
PROJECT LOCATION: /Users/denis/Desktop/Focus/Meal-Plan/
CURRENT STATE: Single-file PWA (index.html, 3420 lines), no service worker

YOUR TASK:
Create a service worker that enables offline functionality with cache-first strategy.

REQUIREMENTS:
1. Create sw.js in project root
2. Cache: index.html, manifest.json, Google Fonts CSS
3. Strategy: Cache-first for static assets, network-first for JSON
4. Register SW in index.html before closing </body>

INTEGRATION POINTS:
- index.html line ~3340 (before DOMContentLoaded init)
- manifest.json (no changes needed)
- data/*.json files (network-first, fallback graceful)

QUALITY STANDARDS:
- Must work on iOS Safari (limited SW support)
- Console log registration status
- Handle SW updates gracefully
- Test: Airplane mode should show cached app
```

#### Deliverables
- [ ] `/sw.js` - Service worker file
- [ ] Registration code added to index.html
- [ ] Tested offline functionality

---

### Assignment 1.2: PWA Icon Generation
**Delegate To:** @pwa-specialist or @html-css-expert
**Priority:** HIGH
**Branch:** `feature/pwa-offline`

#### Context Package

```
PROJECT LOCATION: /Users/denis/Desktop/Focus/Meal-Plan/
CURRENT STATE: manifest.json uses inline SVG icon (works but not ideal)

YOUR TASK:
Create proper PWA icon set for home screen installation.

REQUIREMENTS:
1. Create /icons/ directory
2. Generate icons: 72, 96, 128, 144, 152, 192, 384, 512 px
3. Include maskable icon (512px with safe zone)
4. Use meal/plate emoji or simple food icon
5. Match theme: #09090b background, #10b981 accent

INTEGRATION POINTS:
- Update manifest.json icons array
- Add apple-touch-icon to index.html <head>

QUALITY STANDARDS:
- Icons must render correctly on iOS and Android
- Maskable icon must have 40% safe zone
- Use PNG format for compatibility
```

#### Deliverables
- [ ] `/icons/` directory with all sizes
- [ ] Updated `manifest.json`
- [ ] Apple touch icon in `<head>`

---

## Phase 2: UI Polish

### Assignment 2.1: Expandable Headers
**Delegate To:** @html-css-expert + @javascript-architect
**Priority:** MEDIUM
**Branch:** `feature/ui-polish`

#### Context Package

```
PROJECT LOCATION: /Users/denis/Desktop/Focus/Meal-Plan/
CURRENT STATE: Content cards exist, no accordion/collapse functionality

YOUR TASK:
Add expandable/collapsible functionality to select content cards.

TARGET CARDS (optional expansion):
- Morning Routine card (TODAY section)
- Housemates Cooking Guide (MEALS section)
- Edge Cases FAQ (MEALS section)
- Damage Control Orders (EATING OUT section)

REQUIREMENTS:
1. Add expand/collapse chevron icon to card headers
2. Implement slide-down animation (300ms ease)
3. Save collapsed state to localStorage (optional)
4. Default: expanded on first visit

CSS LOCATION: Lines 11-1613 in index.html
JS LOCATION: Lines 2729-3361 in index.html

QUALITY STANDARDS:
- Animation must be 60fps (use transform/opacity)
- Touch target for toggle >= 44px
- Accessible: aria-expanded attribute
```

#### Deliverables
- [ ] CSS for expand/collapse animation
- [ ] JS toggle functionality
- [ ] ARIA attributes for accessibility

---

### Assignment 2.2: Animation Performance Audit
**Delegate To:** @mobile-optimization
**Priority:** LOW
**Branch:** `feature/ui-polish`

#### Context Package

```
PROJECT LOCATION: /Users/denis/Desktop/Focus/Meal-Plan/
CURRENT STATE: Multiple CSS animations exist, need performance verification

YOUR TASK:
Audit and optimize all animations for 60fps on mobile.

ANIMATIONS TO AUDIT:
- fadeIn (section transitions)
- fillUp (water glasses)
- pulse-today (streak dot)
- confettiFall (success modal)
- modalPop (modal entrance)
- modalBounce (icon bounce)

REQUIREMENTS:
1. Test on throttled CPU (Chrome DevTools)
2. Replace any properties that cause layout/paint
3. Use transform and opacity only where possible
4. Add will-change hints if needed

QUALITY STANDARDS:
- No frame drops below 60fps
- No jank on iPhone SE (smallest viewport)
- Animations disabled for prefers-reduced-motion
```

#### Deliverables
- [ ] Performance audit report
- [ ] Optimized CSS animations
- [ ] prefers-reduced-motion media query

---

## Phase 3: Accessibility & Testing

### Assignment 3.1: Touch Target Audit
**Delegate To:** @mobile-optimization
**Priority:** MEDIUM
**Branch:** `feature/a11y-audit`

#### Context Package

```
PROJECT LOCATION: /Users/denis/Desktop/Focus/Meal-Plan/
CURRENT STATE: Most targets appear compliant, needs verification

YOUR TASK:
Audit all interactive elements for 44px minimum touch target.

ELEMENTS TO CHECK:
- Nav tabs (.nav-tab)
- Day selector buttons (.day-btn)
- Checkboxes (.non-neg-check) - Currently 26px, NEEDS FIX
- Water glasses (.water-glass)
- Builder options (.builder-option)
- Theme color buttons (.theme-color)
- All buttons (.btn, .modal-btn, etc.)

REQUIREMENTS:
1. Measure all touch targets
2. Fix any below 44px (padding, not size if needed)
3. Ensure adequate spacing between targets

QUALITY STANDARDS:
- WCAG 2.1 Level AAA: 44x44px minimum
- No overlapping touch areas
- Adequate spacing (8px minimum)
```

#### Deliverables
- [ ] Touch target audit spreadsheet
- [ ] CSS fixes for undersized targets
- [ ] Verification screenshots

---

### Assignment 3.2: Lighthouse Optimization
**Delegate To:** @code-reviewer
**Priority:** MEDIUM
**Branch:** `feature/a11y-audit`

#### Context Package

```
PROJECT LOCATION: /Users/denis/Desktop/Focus/Meal-Plan/
TARGET: Lighthouse mobile score > 90 in all categories

YOUR TASK:
Run Lighthouse audit and fix any issues preventing 90+ score.

EXPECTED ISSUES:
- PWA score low until service worker added
- Performance may flag large DOM or unused CSS
- Accessibility may flag contrast or labels

REQUIREMENTS:
1. Run Lighthouse in Chrome DevTools (mobile, throttled)
2. Document all issues with scores
3. Fix issues in priority order (biggest impact first)
4. Re-run to verify improvements

QUALITY STANDARDS:
- Performance: > 90
- Accessibility: > 90
- Best Practices: > 90
- SEO: > 90
- PWA: 100% (after Phase 1)
```

#### Deliverables
- [ ] Initial Lighthouse report
- [ ] List of fixes applied
- [ ] Final Lighthouse report showing 90+

---

## Phase 4: Cross-Browser Testing

### Assignment 4.1: Safari iOS Testing
**Delegate To:** @mobile-optimization
**Priority:** HIGH
**Branch:** `main` (after all merges)

#### Context Package

```
YOUR TASK:
Test complete app on Safari iOS and fix any issues.

TEST CHECKLIST:
- [ ] App installs to home screen
- [ ] Offline mode works
- [ ] All animations render correctly
- [ ] Glassmorphism effects work (Safari backdrop-filter)
- [ ] Touch interactions responsive
- [ ] localStorage persists
- [ ] Theme changes apply
- [ ] Water tracker fills correctly
- [ ] Modals open/close properly

KNOWN SAFARI ISSUES TO WATCH:
- backdrop-filter may need -webkit prefix (already added)
- Service worker limitations
- 100vh viewport issues
- Date handling differences
```

#### Deliverables
- [ ] Safari iOS test report
- [ ] Fixes for any Safari-specific bugs
- [ ] Verification video/screenshots

---

## Delegation Timeline

```
Week 1:
├── Day 1-2: Phase 1.1 (Service Worker) - @pwa-specialist
├── Day 2-3: Phase 1.2 (Icons) - @pwa-specialist
└── Day 3: Merge feature/pwa-offline → main

Week 2:
├── Day 1-2: Phase 2.1 (Expandable Headers) - @html-css-expert
├── Day 2: Phase 2.2 (Animation Audit) - @mobile-optimization
└── Day 3: Merge feature/ui-polish → main

Week 3:
├── Day 1: Phase 3.1 (Touch Targets) - @mobile-optimization
├── Day 2: Phase 3.2 (Lighthouse) - @code-reviewer
├── Day 2: Merge feature/a11y-audit → main
└── Day 3: Phase 4.1 (Safari Testing) - @mobile-optimization

Final: All 144 features complete, Lighthouse 90+, production ready
```

---

## Agent Communication Protocol

When delegating, always include:

1. **Project location** - Absolute path
2. **Current state** - What exists now
3. **Specific task** - Clear deliverables
4. **Integration points** - What connects to their work
5. **Quality standards** - How to verify success
6. **Dependencies** - What must exist first

When receiving completed work:

1. **Verify deliverables** - All items present
2. **Test integration** - Works with existing code
3. **Update Integration Status** - Mark features complete
4. **Merge or request changes** - Clear feedback

---

## Notes

- All agents work on branches, never directly on main
- Code review required before merge
- Single-file architecture must be preserved
- Irish context and tone must remain unchanged
- Test on real mobile device before final approval
