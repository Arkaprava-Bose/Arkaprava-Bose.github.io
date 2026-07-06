---
layout: post
title: Continuation Methods for Fold Bifurcations in Stroboscopic Maps
date: 2026-07-07
---
## Problem Statement

We consider a periodically forced discrete map possessing both a stable and an unstable fixed point. As the forcing amplitude increases, 
the stable fixed point eventually collides with the unstable one, leading to a saddle-node (fold) bifurcation of the corresponding stroboscopic map. 
Determining the critical forcing amplitude by directly solving the coupled bifurcation conditions

$$
G(x,\varepsilon) - x = 0, G_y(x,\varepsilon) - 1 = 0
$$

can be challenging, particularly for higher-period stroboscopic maps where the composed map becomes increasingly complicated.
In this post we discuss a computationally inexpensive continuation procedure that accurately estimates the critical forcing amplitude while simultaneously tracking the desired branch of 
stroboscopic fixed points.

---

## Continuation Procedure

To locate the critical forcing amplitude corresponding to a saddle-node bifurcation of the stroboscopic map, a simple parameter continuation scheme can be employed. The method begins from a known stable fixed point of the stroboscopic map at a small forcing amplitude (typically $\varepsilon = 0$) and gradually increases the forcing in small increments. At each continuation step, the previously computed fixed point is used as the initial guess for a nonlinear root solver to obtain the fixed point of the map at a new parameter value. In this manner, the same branch of the stroboscopic fixed points is tracked continuously through parameter space.

The continuation on the same branch is not forced by the algorithm, but because away from bifurcations fixed points vary smoothly with parameters, the previously computed fixed point serves as an excellent initial guess. The local uniqueness guaranteed by the Implicit Function Theorem keeps the solver on the same nearby solution. If multiple branches exist, the method tracks only the one connected to the initial fixed point; discovering other branches requires different initial guesses or more advanced continuation techniques.

The principal advantage of this method is its simplicity. Since successive fixed points are generally close to each other, Newton-type solvers converge rapidly, making the continuation computationally inexpensive. Furthermore, the entire solution branch is obtained as a by-product, which is useful for constructing bifurcation diagrams and for studying scaling behaviour near criticality, where the evaluation of critical forcing amplitudes becomes important. By following a single branch, the method also avoids switching between different stroboscopic phases, ensuring consistency with the analytical composition of the map.

However, simple parameter continuation has an inherent limitation near saddle-node bifurcations. As the fold is approached, the derivative of the solution branch with respect to the continuation parameter diverges,

$$
\frac{dx}{d\varepsilon}
=
-\frac{G_\varepsilon}{G_y-1},
$$

because the bifurcation condition requires

$$
G_y=1.
$$

Consequently, the branch becomes locally vertical in the $(\varepsilon,x)$ plane, and the previous solution is no longer a sufficiently accurate initial guess for the next continuation step. The nonlinear solver therefore loses convergence, even though a nearby solution still exists. Moreover, once the turning point is reached, the branch can no longer be followed by treating $\varepsilon$ as the continuation parameter.

In practice, simple continuation is effective for tracing the stable branch up to the immediate vicinity of the fold. The final continuation point then provides an excellent initial guess for solving the augmented system

$$
\begin{aligned}
G(x,\varepsilon)-x &= 0,\\
G_y(x,\varepsilon)-1 &= 0,
\end{aligned}
$$

which accurately determines the critical point $(x_c,\varepsilon_c)$. For problems involving multiple folds or disconnected branches, pseudo-arclength continuation provides a more robust framework, as it can continue solutions through turning points without relying on the control parameter as the continuation variable.
