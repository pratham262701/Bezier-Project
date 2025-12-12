Interactive Bézier Curve with Physics & Sensor Control

Project Overview
A real-time interactive cubic Bézier curve simulation that responds to mouse movement with spring-damped physics, creating a natural rope-like behavior. All mathematical computations, physics simulations, and rendering are implemented from scratch without external libraries.

Live Demo
Open bezier-curve.html in any modern web browser (Chrome, Firefox, Safari, Edge).

Mathematical Implementation

1. Cubic Bézier Curve Formula
The curve is computed using the parametric cubic Bézier equation:

B(t) = (1-t)³P₀ + 3(1-t)²tP₁ + 3(1-t)t²P₂ + t³P₃

Where:
- t ∈ [0, 1] - parameter representing position along curve
- P₀, P₃ - fixed endpoints (red control points)
- P₁, P₂ - dynamic control points (yellow, respond to input)

Implementation Details:
- Curve sampled at dt = 0.01 intervals for smooth rendering
- Binomial expansion computed manually without using built-in Bézier APIs
- Each point calculated by weighted blend of all four control points

2. Tangent Vector Calculation
Tangents are derived from the first derivative of the Bézier function:

B'(t) = 3(1-t)²(P₁-P₀) + 6(1-t)t(P₂-P₁) + 3t²(P₃-P₂)

Purpose:
- Shows instantaneous direction of curve at any point
- Visualized as cyan arrows at regular intervals (dt = 0.15)
- Normalized to consistent length for clarity

Implementation:
- Derivative computed from first principles (no automatic differentiation)
- Vectors normalized using length = √(x² + y²)
- Drawn perpendicular to curve path

Physics Model

Spring-Damping System
Control points P₁ and P₂ use a spring-damper model for smooth, natural motion:

acceleration = -k × (position - target) - damping × velocity
velocity += acceleration × dt
position += velocity × dt


Parameters:
- Spring constant (k): 2.5 - Controls responsiveness (higher = snappier)
- Damping coefficient: 0.7 - Reduces oscillation (higher = less bouncy)
- Integration: Euler method with adaptive timestep (dt)

Why This Model?
- Mimics real-world rope/spring behavior
- Creates smooth, organic movement
- Prevents jerky, instant position changes
- Allows natural overshoot and settling

Design Rationale:
Initially used k=0.5, damping=0.85 for subtle movement, but increased to k=2.5, damping=0.7 for more visible, responsive interaction suitable for demonstration purposes.

Input Handling

Mouse Control (Web Version)
- P₁ (top-left control point):
  - Tracks mouse position with 60% scaling
  - Moves in same direction as cursor
  - offset = (mouse - center) × 0.6

- P₂ (bottom-right control point):
  - Horizontal movement follows mouse (48% scaling)
  - Vertical movement inverted (creates wave effect)
  - offsetX = (mouse.x - center.x) × 0.48
  - offsetY = -(mouse.y - center.y) × 0.48

Interaction Design:
- Fixed endpoints (P₀, P₃) maintain curve structure
- Dynamic points create natural S-curve deformation
- Opposite vertical movement produces rope-swinging effect

Code Architecture

Module Organization

1. Math Module (Vec2 class)
- Vector operations: add, subtract, multiply, normalize
- Length calculation for normalization
- Pure mathematical operations without side effects

2. Physics Module (ControlPoint class)
- Position, velocity, target state management
- Spring-damping update loop
- Fixed/dynamic point differentiation

3. Rendering Module (BezierRenderer class)
- Canvas initialization and management
- Curve rendering with gradient visualization
- Tangent line drawing
- Control point visualization
- Animation loop at 60+ FPS

Key Functions

javascript
bezierPoint(t, p0, p1, p2, p3)     // Computes curve position at parameter t
bezierTangent(t, p0, p1, p2, p3)   // Computes derivative/tangent at t
ControlPoint.update(dt, k, damping) // Physics step integration
BezierRenderer.animate()            // Main render loop with RAF


Design Choices

Why Not Use Built-in APIs?
- Educational Value: Understanding the mathematics deeply
- Control: Fine-tuned behavior for specific requirements
- Transparency: Every calculation is visible and modifiable
- Performance: Optimized for this specific use case

Visual Design
- Gradient curve: Pink → Purple → Blue for visual appeal
- Glow effects: Shadows on curve and tangents for depth
- Color coding:
  - Red = Fixed endpoints
  - Yellow = Dynamic control points
  - Cyan = Tangent vectors
- Dark background: Enhances neon aesthetic and readability

Performance Optimizations
- Single requestAnimationFrame loop
- Adaptive timestep prevents physics explosions
- FPS counter for performance monitoring
- Efficient vector operations without object creation in hot paths

Features Implemented

✅ Manual Bézier Mathematics - No UIBezierPath or library usage  
✅ Tangent Computation - First derivative implementation  
✅ Spring-Damping Physics - Natural motion simulation  
✅ Real-time Interaction - Mouse-responsive control points  
✅ 60+ FPS Performance - Smooth rendering with monitoring  
✅ Visual Feedback - Control points, tangents, gradient curve  

Running the Project

Requirements
- Modern web browser (Chrome, Firefox, Safari, Edge)
- No server or dependencies required

Instructions
1. Open bezier-curve.html in your browser
2. Move your mouse over the canvas
3. Watch the curve respond with spring-damped physics
4. Observe tangent vectors showing curve direction

Technical Specifications

- Language: Vanilla JavaScript (ES6+)
- Rendering: HTML5 Canvas 2D Context
- Animation: requestAnimationFrame for smooth 60 FPS
- Math: Custom vector class with manual Bézier implementation
- Physics: Euler integration with spring-damper model
- No External Libraries: Pure implementation from scratch

Future Enhancements (Optional)

- Multi-touch support for mobile devices
- Gyroscope integration for iOS native version
- Adjustable spring constants via UI controls
- Multiple curve segments (spline chains)
- Export curve data as SVG path

Author
Pratham Agarwal

This project demonstrates understanding of:
- Parametric curve mathematics
- Physics simulation and numerical integration
- Real-time graphics rendering
- Event-driven programming
- Performance optimization for interactive applications

All code written from first principles without reliance on graphics or physics libraries.
