# UI/UX Design Guidelines - Nia Match Platform

## Design Philosophy

### Core Principles
- **Islamic Values First**: Every design decision respects Islamic principles of modesty and family involvement
- **Simplicity & Clarity**: Clean, uncluttered interface that reduces cognitive load
- **Trust & Security**: Visual cues that reinforce platform reliability and data protection
- **Accessibility**: Inclusive design supporting users with diverse abilities and technical literacy
- **Cultural Sensitivity**: Design patterns that resonate with global Muslim communities

### Visual Identity
- **Calming & Respectful**: Soft, professional color palette
- **Modern Islamic Aesthetic**: Contemporary design with subtle Islamic geometric patterns
- **Typography**: Clear, readable fonts supporting Arabic/RTL text if needed
- **Imagery**: No personal photos; focus on Islamic art, calligraphy, and geometric patterns

---

## Main Application Layout

### 1. Dashboard Layout Structure

```
┌─────────────────────────────────────────────────────────┐
│ HEADER: Logo | Navigation | Trial Status | Profile Menu │
├─────────────────────────────────────────────────────────┤
│ SIDEBAR (Desktop)    │ MAIN CONTENT AREA               │
│ - Dashboard          │                                 │
│ - New Matches        │ [Dynamic Content Based on       │
│ - Conversations      │  Selected Navigation]           │
│ - Profile Settings   │                                 │
│ - Help & Support     │                                 │
│                      │                                 │
├─────────────────────────────────────────────────────────┤
│ FOOTER: Privacy | Terms | Support | Islamic Guidelines │
└─────────────────────────────────────────────────────────┘
```

### 2. Mobile-First Responsive Design

**Mobile Layout (320px - 768px)**:
- Collapsible hamburger navigation
- Bottom tab bar for main sections
- Full-width cards for matches
- Swipe gestures for match decisions

**Tablet Layout (768px - 1024px)**:
- Sidebar navigation toggleable
- Two-column content layout
- Larger touch targets
- Modal dialogs for detailed views

**Desktop Layout (1024px+)**:
- Fixed sidebar navigation
- Three-column layout for matches view
- Hover states and keyboard navigation
- Multi-panel conversations view

---

## Key UI Components

### 1. Match Card Component

```
┌─────────────────────────────────────────┐
│ COMPATIBILITY BADGE: 92% Match         │
├─────────────────────────────────────────┤
│ Enneagram Type: The Helper (Type 2)    │
│ ┌─────────────┐ ┌─────────────────────┐ │
│ │ STRENGTHS   │ │ DEAL-BREAKERS       │ │
│ │ • Caring    │ │ ✅ Same Aqeedah     │ │
│ │ • Empathetic│ │ ✅ Family-Oriented  │ │
│ │ • Supportive│ │ ✅ Wants Children   │ │
│ └─────────────┘ └─────────────────────┘ │
├─────────────────────────────────────────┤
│ [ACCEPT] [SAVE FOR LATER] [REJECT]      │
└─────────────────────────────────────────┘
```

**Design Features**:
- Color-coded compatibility percentage
- Clear iconography for deal-breakers
- Islamic-inspired card borders
- Accessible button sizing (minimum 44px touch targets)

### 2. Group Chat Interface

```
┌─────────────────────────────────────────┐
│ CHAT HEADER                             │
│ 👥 Family Introduction                  │
│ Online: Ahmad, Fatima, Uncle Ali, Aunt Sarah │
├─────────────────────────────────────────┤
│ ISLAMIC GUIDELINES BANNER               │
│ 📖 Remember to maintain Islamic etiquette │
├─────────────────────────────────────────┤
│ MESSAGE AREA                            │
│                                         │
│ [Uncle Ali]: Assalamu alaikum...        │
│ [You]: Wa alaikum assalam...           │
│ [Aunt Sarah]: We appreciate...          │
│                                         │
├─────────────────────────────────────────┤
│ [Message Input] [📎] [Send]             │
└─────────────────────────────────────────┘
```

**Design Features**:
- Clear participant identification
- Islamic etiquette reminders
- File attachment (documents only)
- Message status indicators
- Typing indicators for active participants

### 3. Onboarding Flow Design

#### Step 1: Welcome Screen
```
┌─────────────────────────────────────────┐
│           🕌 NIA MATCH                  │
│                                         │
│    "Finding your life partner the      │
│         Islamic way"                    │
│                                         │
│  🤝 Personality-based matching          │
│  👨‍👩‍👧‍👦 Wali involvement required        │
│  ✅ 100% Halal communication            │
│                                         │
│        [Start Your Journey]             │
│                                         │
│   Already have an account? [Sign In]   │
└─────────────────────────────────────────┘
```

#### Step 2: Enneagram Test Interface
```
┌─────────────────────────────────────────┐
│ PROGRESS: ████████░░ (8/10 Complete)    │
├─────────────────────────────────────────┤
│ Question 8 of 10                        │
│                                         │
│ "When making important decisions,       │
│  I tend to..."                          │
│                                         │
│ ○ Rely on my gut feeling               │
│ ○ Analyze all possible outcomes        │
│ ○ Seek advice from trusted friends     │
│ ○ Consider what's best for others      │
│                                         │
├─────────────────────────────────────────┤
│          [Previous] [Next]              │
└─────────────────────────────────────────┘
```

---

## User Interaction Patterns

### 1. Match Decision Flow
**Swipe Gestures (Mobile)**:
- Swipe right: Accept match
- Swipe left: Reject match
- Swipe up: Save for later
- Tap: View detailed profile

**Button Actions (Desktop)**:
- Large, clearly labeled action buttons
- Confirmation dialogs for irreversible actions
- Keyboard shortcuts for power users

### 2. Navigation Patterns
**Primary Navigation**:
- Dashboard (Home icon)
- New Matches (Heart icon)
- Conversations (Chat bubble icon)
- Profile (User icon)
- Settings (Gear icon)

**Secondary Navigation**:
- Breadcrumb navigation for multi-step processes
- Back buttons with clear return paths
- Progress indicators for onboarding flows

### 3. Feedback & Status Communication
**Visual Feedback**:
- Loading states with Islamic geometric animations
- Success messages with appropriate Islamic expressions
- Error states with helpful guidance
- Empty states with encouraging messaging

---

## Accessibility Considerations

### 1. Visual Accessibility
- **Color Contrast**: WCAG AA compliant (4.5:1 minimum ratio)
- **Font Sizes**: Minimum 16px for body text, scalable up to 200%
- **Focus Indicators**: Clear keyboard navigation highlights
- **Alternative Text**: Descriptive alt text for all images and icons

### 2. Motor Accessibility
- **Touch Targets**: Minimum 44px for mobile interactions
- **Keyboard Navigation**: Full functionality without mouse
- **Voice Control**: Compatible with speech recognition software
- **Gesture Alternatives**: Button alternatives for all swipe actions

### 3. Cognitive Accessibility
- **Clear Language**: Simple, jargon-free instructions
- **Progress Indicators**: Always show current step and total steps
- **Error Prevention**: Input validation with helpful messages
- **Consistent Patterns**: Reusable UI patterns throughout app

### 4. Cultural & Language Accessibility
- **RTL Support**: Right-to-left reading for Arabic users
- **Cultural Icons**: Familiar symbols for global Muslim audience
- **Time Zones**: Prayer time awareness in notifications
- **Calendar Integration**: Islamic calendar reference where relevant

---

## Responsive Design Approach

### 1. Mobile-First Strategy
**Core Principles**:
- Design starts with mobile constraints
- Progressive enhancement for larger screens
- Touch-first interaction design
- Offline functionality for key features

### 2. Breakpoint System
```scss
// Mobile Small
@media (min-width: 320px) { /* Phone */ }

// Mobile Large  
@media (min-width: 480px) { /* Large Phone */ }

// Tablet
@media (min-width: 768px) { /* Tablet Portrait */ }

// Desktop Small
@media (min-width: 1024px) { /* Tablet Landscape/Small Desktop */ }

// Desktop Large
@media (min-width: 1200px) { /* Desktop */ }
```

### 3. Content Strategy
**Progressive Disclosure**:
- Essential information visible first
- Secondary details available on demand
- Advanced features tucked behind "More" options
- Context-sensitive help and tooltips

---

## Design System Components

### 1. Color Palette
```scss
// Primary Colors
$primary-green: #2D5A4F;    // Islamic green (trust, peace)
$primary-gold: #C49B61;     // Islamic gold (warmth, wisdom)
$primary-navy: #1C3A57;     // Deep blue (stability, trust)

// Secondary Colors
$light-sage: #E8F2EF;       // Background tints
$warm-cream: #FAF7F2;       // Card backgrounds
$soft-gray: #6B7280;        // Text secondary

// Status Colors
$success: #10B981;          // Match acceptance
$warning: #F59E0B;          // Trial expiration
$error: #EF4444;            // Validation errors
$info: #3B82F6;             // General information
```

### 2. Typography Scale
```scss
// Font Families
$font-primary: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
$font-heading: 'Crimson Text', serif; // For headings, elegant
$font-arabic: 'Noto Sans Arabic', sans-serif; // For Arabic text

// Font Sizes (Mobile-first)
$text-xs: 0.75rem;    // 12px - captions
$text-sm: 0.875rem;   // 14px - small text
$text-base: 1rem;     // 16px - body text
$text-lg: 1.125rem;   // 18px - large body
$text-xl: 1.25rem;    // 20px - small headings
$text-2xl: 1.5rem;    // 24px - section headings
$text-3xl: 1.875rem;  // 30px - page headings
```

### 3. Spacing System
```scss
// Base unit: 4px
$space-1: 0.25rem;   // 4px
$space-2: 0.5rem;    // 8px
$space-3: 0.75rem;   // 12px
$space-4: 1rem;      // 16px
$space-6: 1.5rem;    // 24px
$space-8: 2rem;      // 32px
$space-12: 3rem;     // 48px
$space-16: 4rem;     // 64px
```

---

## Special Considerations

### 1. Islamic Design Elements
- **Geometric Patterns**: Subtle background patterns inspired by Islamic art
- **Calligraphy**: Tasteful use of Arabic calligraphy for Islamic phrases
- **Prayer Time Integration**: Respect for prayer times in notification scheduling
- **Hijri Calendar**: Support for Islamic calendar dates alongside Gregorian

### 2. Privacy & Modesty Design
- **No Personal Photos**: Profile represented by Enneagram symbols
- **Masked Information**: Limited personal details visible until mutual match
- **Wali Visibility**: Clear indicators of guardian involvement
- **Secure Communication**: Visual indicators of end-to-end chat protection

### 3. Performance Optimization
- **Progressive Web App**: App-like experience on mobile browsers
- **Lazy Loading**: Images and content loaded as needed
- **Offline Support**: Basic functionality without internet connection
- **Fast Loading**: Target sub-2 second load times on 3G networks

This design system ensures a respectful, accessible, and user-friendly experience that honors Islamic values while providing modern functionality for finding compatible marriage partners. 