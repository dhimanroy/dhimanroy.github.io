---
title: Scheme code for extacting xy plot data
weight: 23
menu:
  codes:
    name: For extacting xy plot data
    identifier: codes-scheme-extractPlotData
    parent: codes-scheme
    weight: 23
---
{{< note title="Scheme code for extacting xy plot data" >}}
```scheme
;==========================================
 ; Title:  Data Extract for XY plots
 ; Author: Dhiman Roy, ME-14, BUET
 ; Date:   February 08, 2021
 ; Licensed under Creative Commons.
 ;==========================================

(newline)
(display "Generating VERTICAL LINES...")(newline)
(newline)
;(define avg "Area-Weighted Average")
(define ramp_x-cor (list 0.060 0.070))
(define ramp_y_1-cor (list -0.018 -0.018))
(define ramp_y_2-cor (list -0.033 -0.033))

(define lip_x-cor (list 0.060 0.070))
(define lip_y_1-cor (list -0.018 -0.018))
(define lip_y_2-cor (list -0.033 -0.033))

(
    Do ((i 1 (+ i 1))) ((> i (length ramp_x-cor)))
    (Ti-menu-load-string (format #f "surface/line-surface line_ramp_~a ~a ~a ~a ~a" i (list-ref ramp_x-cor (- i 1)) (list-ref ramp_y_1-cor (- i 1)) (list-ref ramp_x-cor (- i 1)) (list-ref ramp_y_2-cor (- i 1))))
    (newline)
    (Ti-menu-load-string (format #f "plot/plot yes ramp_x-vel_~a.dat no yes 0 1 no no x-velocity line_ramp_~a ()" (* 10000 (list-ref ramp_x-cor (- i 1))) i))
    (Ti-menu-load-string (format #f "plot/plot yes ramp_temp_~a.dat no yes 0 1 no no temperature line_ramp_~a ()" (* 10000 (list-ref ramp_x-cor (- i 1))) i))
)

(newline)
(newline)

(
    Do ((i 1 (+ i 1))) ((> i (length lip_x-cor)))
    (Ti-menu-load-string (format #f "surface/line-surface line_lip_~a ~a ~a ~a ~a" i (list-ref lip_x-cor (- i 1)) (list-ref lip_y_1-cor (- i 1)) (list-ref lip_x-cor (- i 1)) (list-ref lip_y_2-cor (- i 1))))
    (newline)
    (Ti-menu-load-string (format #f "plot/plot yes lip_x-vel_~a.dat no yes 0 1 no no x-velocity line_lip_~a ()" (* 10000 (list-ref lip_x-cor (- i 1))) i))
)

(newline)
(newline)

(
    Do ((i 1 (+ i 1))) ((> i 51))
    (display "*")
)
(newline)
(display "Vertical lines are generated.")(newline)
;(display "Monitors are created..")(newline)
(display "Files are saved in current working directory...")(newline)
(
    Do ((i 1 (+ i 1))) ((> i 51))
    (display "*")
)
(newline)
(newline)
```
{{< /note >}}
