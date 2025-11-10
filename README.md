# Springy Cubic Bézier Curve – Interactive Web Demo

This project shows a moving cubic Bézier curve that reacts to the user's mouse movements.  
The curve behaves like a loose rope that bends and settles instead of instantly snapping into place.  
The animation and motion logic are written directly in JavaScript on an HTML canvas — nothing here relies on built-in curve or physics libraries.

---

## How the Curve Is Formed (Math Overview)

A cubic Bézier curve uses four points arranged in order:

- **P₀**: starting point (does not move)
- **P₁**: first control point (moves)
- **P₂**: second control point (moves)
- **P₃**: end point (does not move)

The position of the curve for a value `t` between 0 and 1 is found using:

\[
B(t) = (1 - t)^3P_0 + 3(1 - t)^2tP_1 + 3(1 - t)t^2P_2 + t^3P_3
\]

In simpler terms:  
`t` slides from left to right, and the equation blends the four points to form a smooth path.  
The script draws the curve by increasing `t` in small steps and connecting the resulting points.

### Showing the Curve's Direction

To show the direction the curve is moving at certain points, we use the derivative:

\[
B'(t) = 3(1-t)^2(P_1-P_0) + 6(1-t)t(P_2-P_1) + 3t^2(P_3-P_2)
\]

This gives a **direction vector**.  
We scale it to a short line so the curve doesn’t look cluttered.

---

## How the Motion Works (Spring + Damping Idea)

The points **P₀** and **P₃** stay in place, so they act like the two hooks holding our “rope.”

The two control points, **P₁** and **P₂**, chase target positions influenced by the mouse.  
However, instead of moving directly to the target, they move through a **spring-like motion**:

\[
a = -k(x - x_{\text{target}}) - d \cdot v
\]

Meaning:

| Symbol | Explanation |
|-------|------------|
| `x` | The point’s current position |
| `x_target` | The place it wants to move toward |
| `v` | Current velocity of the point |
| `k` | Spring force (stronger = moves back quicker) |
| `d` | Damping (reduces bouncing) |
| `a` | The amount the point accelerates |

Each frame, the velocity and position are updated a little at a time.  
This gives a motion that:
- starts fast,
- overshoots slightly,
- and settles smoothly.

It ends up feeling *natural*, like moving fabric or rope.

---

## Design Choices and Why They Were Used

| Choice | Reason |
|-------|--------|
| Manual Bézier math instead of canvas curves | Makes the curve logic visible and controllable. |
| Spring-damper movement instead of instant snap | Creates a soft, realistic feel. |
| Tangent markers | Makes the curve’s direction easy to understand. |
| Dashed line between control points | Helps visualize how control points influence the final shape. |
| requestAnimationFrame loop | Keeps animation smooth and efficient. |

I aimed for the visuals to teach *why* the curve bends the way it does, not just show the final line.

---

## How to Try It

1. Open **index.html** in any modern browser.
2. Move your mouse across the screen — the curve will react.
3. Drag near P₁ or P₂ to pull the rope directly.
4. Use the sliders to adjust the "stiffness" and "damping" if you want to play with how the rope behaves.

No installation steps or external libraries are needed.

---

## Project Files

