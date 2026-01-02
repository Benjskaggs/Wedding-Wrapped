# Wedding Wrapped

## Project Overview

**Ben and Leah Wrapped** is a mobile-optimized, interactive web application that tells Ben and Leah's love story in a Spotify Wrapped-inspired card format. The site features a swipeable/tappable card interface with flip animations to reveal different moments in their relationship journey.

## Tech Stack

- **Frontend**: Vanilla HTML, CSS, and JavaScript
- **Styling**: Custom CSS with modern features including:
  - CSS Grid and Flexbox layouts
  - CSS transforms and 3D card flips
  - CSS gradients and animations
  - CSS custom properties for safe area insets
- **Design System**:
  - Font: Montserrat (Google Fonts)
  - Color Palette: Cornflower Blue (#6495ED), Light Pink (#FFB6C1), Peach Puff (#FFDAB9)
  - Mobile-first responsive design

## Project Structure

```
Wedding-Wrapped/
├── index.html              # Main application file (single-page app)
├── assets/
│   └── first-date-bowls.jpeg  # Photo from first date at Color Me Mine
├── README.md              # Basic project readme
└── claude.md              # This file - comprehensive project documentation
```

## Features

### Current Implementation (v1.0)

1. **Card Flip Interface**
   - Interactive 3D card flip animation on tap/click
   - Front side shows event photo and description
   - Back side reveals additional story details
   - Smooth cubic-bezier transitions

2. **Event Story: First Date**
   - Front: Image of painted ice cream bowls from Color Me Mine
   - Story: How Ben asked Leah out after 4 years (with help from friends)
   - Back: The aftermath - Leah declined further dates, Ben threw away the bowls 5 months later

3. **Mobile Optimizations**
   - Viewport meta tags for proper mobile scaling
   - Safe area insets for notched devices (iPhone X+)
   - Touch-optimized interactions
   - Responsive typography and spacing
   - Height-based media queries for different screen sizes
   - Apple-specific web app meta tags

4. **Progress Indicator**
   - Shows 5 total story cards planned
   - Currently at card 1/5
   - Visual dots with active state highlighting

### Planned Features

Based on the progress indicator showing 5 dots, the plan includes **4 additional story cards** to complete the narrative:
- Card 2: [To be implemented]
- Card 3: [To be implemented]
- Card 4: [To be implemented]
- Card 5: [To be implemented]

## Design System

### Color Palette

```css
Primary Blue: #6495ED (Cornflower Blue)
Light Pink: #FFB6C1
Peach: #FFDAB9
Light Blue: #87CEEB
Deep Pink: #DB7093

Backgrounds:
- White: #ffffff
- Light Peach: #fdf8f7
- Lighter Pink: #fff5f5

Text:
- Dark Gray: #333333, #444444, #555555, #5a4a4a
```

### Typography

- **Font Family**: Montserrat (weights: 400, 600, 700, 800)
- **Fallbacks**: -apple-system, BlinkMacSystemFont, sans-serif
- **Header**: 1.6rem (1.4rem on small screens), weight 800
- **Body Text**: 1.05rem (0.95rem on small screens), weight 400
- **Tags**: 0.7rem, weight 700, uppercase with letter-spacing

### Layout Breakpoints

- **Mobile**: Default (< 500px width)
- **Small height devices**: < 700px height (compressed spacing)
- **Tablet/Desktop**: ≥ 500px width (centered max-width 420px)

## Technical Implementation Notes

### Card Flip Mechanism

The 3D card flip uses CSS transforms with `transform-style: preserve-3d` and `backface-visibility: hidden`. The flip is triggered by a simple class toggle:

```javascript
onclick="this.classList.toggle('flipped')"
```

When `.flipped` class is added, the card rotates 180 degrees on the Y-axis.

### Mobile-First Approach

The design prioritizes mobile devices with:
- Full viewport height utilization
- `-webkit-fill-available` for iOS Safari compatibility
- `env(safe-area-inset-*)` for notched devices
- Disabled user scaling for app-like experience
- Overflow hidden to prevent scrolling bounce

### Progressive Enhancement

- Base HTML structure works without JavaScript
- CSS handles all animations and transitions
- Minimal JavaScript footprint (single inline onclick)

## Development Workflow

### Current State

- **Branch**: `main`
- **Status**: Clean working tree
- **Recent Activity**: Merged PR #1 adding event highlights feature

### Git History

```
3237460 - Merge pull request #1 from Benjskaggs/agent-with-event-highlights-c994
ef0c8df - first commit
b57c86b - Create Mobile-Optimized Website "Ben and Leah Wrapped" with Event Highlights
ebf61fe - first commit
```

## Future Enhancements

### Immediate Next Steps

1. **Add Remaining Story Cards** (Cards 2-5)
   - Implement card navigation/swiping
   - Add photos for each milestone
   - Write compelling front/back stories

2. **Navigation System**
   - Swipe gestures for mobile
   - Arrow buttons for desktop
   - Keyboard navigation support
   - URL routing for direct card access

3. **Animation Polish**
   - Entry animations for each card
   - Transition effects between cards
   - Confetti or particle effects for special moments

### Potential Features

- **Audio**: Background music or sound effects
- **Share Functionality**: Social media sharing per card
- **Timeline View**: Overview of all moments
- **Video Support**: Embed videos in certain cards
- **Guestbook**: Collection of messages from wedding guests
- **RSVP Integration**: Link to wedding RSVP system
- **Photo Gallery**: Expandable photo collections
- **Date Countdown**: Days until wedding
- **Save the Date**: Downloadable calendar invites

## Deployment

The project is a static website that can be deployed to:
- GitHub Pages
- Netlify
- Vercel
- AWS S3 + CloudFront
- Any static hosting service

No build process or server-side rendering required.

## Browser Compatibility

Tested/designed for:
- iOS Safari (primary target)
- Chrome Mobile
- Modern desktop browsers

Uses modern CSS features:
- CSS Grid & Flexbox
- CSS Custom Properties
- Transform 3D
- Backdrop filters
- Gradient text

Consider adding fallbacks or autoprefixer for broader compatibility.

## Performance Considerations

- **Images**: Currently using JPEG, consider WebP with fallbacks
- **Fonts**: Using Google Fonts with preconnect for faster loading
- **CSS**: All styles inline (consider extracting to separate file for caching)
- **JavaScript**: Minimal inline JS (could benefit from external file with async/defer)

## Accessibility Notes

Current accessibility could be improved:
- Add ARIA labels for card flip interaction
- Ensure keyboard navigation works
- Add focus styles for interactive elements
- Consider reduced-motion preferences for animations
- Improve alt text descriptions for images
- Add proper heading hierarchy

## License & Credits

- **Project**: Ben and Leah's Wedding Wrapped
- **Created**: 2025
- **Design Inspiration**: Spotify Wrapped annual recap feature
