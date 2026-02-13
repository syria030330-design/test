# Homepage Implementation Plan

## Overview
Create a new `index.html` homepage that links to `snake.html` and `block.html` games with improved design and user experience.

## Design Improvements from Existing code.html

### 1. Visual Enhancements
- **Animated Background Elements**: Floating game icons with animation delays
- **Enhanced Color Scheme**: Added accent-blue color and gradient text effects
- **Improved Shadows**: Added glow effects and enhanced hover states
- **Smooth Animations**: Float, pulse-glow, and bounce-slow animations

### 2. Structural Improvements
- **Better Information Architecture**: Clear sections (Hero, Games, Stats, Coming Soon)
- **Enhanced Game Cards**: Detailed feature lists, control guides, and visual indicators
- **Interactive Elements**: Hover effects, smooth transitions, and confetti animations
- **Responsive Design**: Mobile-first approach with breakpoint optimizations

### 3. User Experience Improvements
- **Smooth Scrolling**: JavaScript for anchor link navigation
- **Theme Toggle**: Dark/light mode switch with persistent button
- **Interactive Feedback**: Confetti effects on game button clicks
- **Clear CTAs**: Prominent "Play Now" buttons with visual hierarchy

## Technical Implementation Details

### File Structure
```
index.html          # Main homepage
snake.html          # Neon Snake game (existing)
block.html          # Block Puzzle/Tetris game (existing)
plans/              # Planning documents
```

### Dependencies
- **Tailwind CSS**: Via CDN with custom configuration
- **Google Fonts**: Fredoka (sans-serif) and Titan One (display)
- **Material Icons**: For consistent iconography
- **Custom JavaScript**: For interactive features

### Key Features to Implement

#### 1. Navigation System
- Fixed header with logo and navigation links
- Smooth scrolling to sections
- Mobile-responsive menu (hidden on small screens)

#### 2. Game Cards
- **Snake Game Card**:
  - Links to `snake.html`
  - Features: Neon visuals, mobile controls, high scores
  - Controls: Arrow keys, touch controls, space to pause
  - Visual indicators: Arcade, Classic, High Score badges

- **Block Puzzle Card**:
  - Links to `block.html`
  - Features: Next piece preview, level system, score tracking
  - Controls: Arrow keys, Z/X rotation, space for hard drop
  - Visual indicators: Puzzle, Strategy, Addictive badges

#### 3. Interactive Elements
- Theme toggle button (bottom-right corner)
- Confetti animation on game button clicks
- Hover effects on all interactive elements
- Animated background elements

#### 4. Responsive Design
- Mobile-first CSS approach
- Grid layouts that adapt to screen size
- Touch-friendly buttons and controls
- Optimized typography scaling

## Implementation Steps

### Step 1: Create index.html Structure
1. Basic HTML5 boilerplate with proper meta tags
2. Include external dependencies (Tailwind, Fonts, Icons)
3. Create header with logo and navigation
4. Build hero section with call-to-action buttons
5. Implement game cards section with links to games
6. Add stats and coming soon sections
7. Create footer with social links

### Step 2: Add Interactive JavaScript
1. Smooth scrolling for anchor links
2. Theme toggle functionality
3. Confetti animation on game button clicks
4. Enhanced hover effects for game cards

### Step 3: Style Enhancements
1. Custom Tailwind configuration with extended colors
2. CSS animations for floating elements
3. Gradient text effects
4. Responsive breakpoints

### Step 4: Testing
1. Verify all links work correctly
2. Test responsive design on different screen sizes
3. Check browser compatibility
4. Validate HTML structure

## Code Snippets for Critical Components

### Theme Toggle JavaScript
```javascript
const themeToggle = document.createElement('button');
themeToggle.innerHTML = '<span class="material-symbols-outlined">dark_mode</span>';
themeToggle.className = 'fixed bottom-6 right-6 w-12 h-12 rounded-full bg-white dark:bg-gray-800 border-2 border-green-100 dark:border-gray-700 flex items-center justify-center text-gray-500 hover:bg-primary hover:text-white hover:border-primary transition-all shadow-lg z-50';

themeToggle.addEventListener('click', () => {
    const html = document.documentElement;
    if (html.classList.contains('light')) {
        html.classList.remove('light');
        html.classList.add('dark');
        themeToggle.innerHTML = '<span class="material-symbols-outlined">light_mode</span>';
    } else {
        html.classList.remove('dark');
        html.classList.add('light');
        themeToggle.innerHTML = '<span class="material-symbols-outlined">dark_mode</span>';
    }
});
```

### Confetti Animation
```javascript
document.querySelectorAll('a[href="snake.html"], a[href="block.html"]').forEach(button => {
    button.addEventListener('click', (e) => {
        for (let i = 0; i < 20; i++) {
            setTimeout(() => {
                const confetti = document.createElement('div');
                confetti.className = 'fixed w-2 h-2 rounded-full z-50 pointer-events-none';
                confetti.style.backgroundColor = button.href.includes('snake.html') ? '#4ade80' : '#a855f7';
                // ... animation logic
            }, i * 30);
        }
    });
});
```

## Success Criteria
1. ✅ Homepage loads without errors
2. ✅ All navigation links work correctly
3. ✅ Game cards link to respective game pages
4. ✅ Responsive design works on mobile, tablet, and desktop
5. ✅ Interactive features (theme toggle, animations) function properly
6. ✅ Visual design is consistent and appealing

## Next Steps
1. Switch to Code mode to implement the HTML file
2. Test implementation in browser
3. Make adjustments based on user feedback
4. Consider adding more games in the future