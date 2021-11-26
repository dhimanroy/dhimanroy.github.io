---
title: Scheme code for creating points and monitors
date: 2021-02-07
weight: 31
menu:
  codes:
    name: For creating points and monitors
    identifier: codes-scheme-createPointsAndMonitors
    parent: codes-scheme
    weight: 31
---
{{< note title="Scheme code for creating points and monitors" >}}
```scheme
;==========================================
 ; Title:  Points and Monitors Generator
 ; Author: Dhiman Roy, ME-14, BUET
 ; Date:   February 07, 2021
 ; Licensed under Creative Commons.
 ;==========================================

(newline)
(display "Generating points and creating monitors...")(newline)
(newline)
(define avg "Area-Weighted Average")
(define x-cor (list of x co-ordinates)) ;hidden data
(define y-cor (list of y co-ordinates)) ;hidden data
(
    Do ((i 1 (+ i 1))) ((> i (length x-cor)))
    (Ti-menu-load-string (format #f "surface/point-surface point_~a_~a ~a ~a" (* 10000 (list-ref x-cor (- i 1))) (* 10000 (list-ref y-cor (- i 1))) (list-ref x-cor (- i 1)) (list-ref y-cor (- i 1))))
    (newline)
    (Ti-menu-load-string (format #f "solve/monitors/surface/set-monitor pressure-monitor-~a avg pressure point_~a_~a () no no yes pres_~a_~a.txt 1 yes flow-time" i (* 10000 (list-ref x-cor (- i 1))) (* 10000 (list-ref y-cor (- i 1))) (* 10000 (list-ref x-cor (- i 1))) (* 10000 (list-ref y-cor (- i 1)))))
    (newline)
    (Ti-menu-load-string (format #f "solve/monitors/surface/set-monitor temperature-monitor-~a avg temperature point_~a_~a () no no yes temp_~a_~a.txt 1 yes flow-time" i (* 10000 (list-ref x-cor (- i 1))) (* 10000 (list-ref y-cor (- i 1))) (* 10000 (list-ref x-cor (- i 1))) (* 10000 (list-ref y-cor (- i 1)))))
    (newline)
    ;(Ti-menu-load-string (format #f "solve/monitors/surface/set-monitor x-velocity-monitor-~a avg x-velocity point_~a_~a () no no yes xvel_~a_~a.txt 1 yes flow-time" i (* 10000 (list-ref x-cor (- i 1))) (* 10000 (list-ref y-cor (- i 1))) (* 10000 (list-ref x-cor (- i 1))) (* 10000 (list-ref y-cor (- i 1)))))
    (newline)
    (Ti-menu-load-string (format #f "solve/monitors/surface/set-monitor y-velocity-monitor-~a avg y-velocity point_~a_~a () no no yes yvel_~a_~a.txt 1 yes flow-time" i (* 10000 (list-ref x-cor (- i 1))) (* 10000 (list-ref y-cor (- i 1))) (* 10000 (list-ref x-cor (- i 1))) (* 10000 (list-ref y-cor (- i 1)))))
)
(newline)
(newline)
(
    Do ((i 1 (+ i 1))) ((> i 51))
    (display "*")
)
(newline)
(display "Points are generated.")(newline)
(display "Monitors are created..")(newline)
(display "Files will be saved in current working directory...")(newline)
(
    Do ((i 1 (+ i 1))) ((> i 51))
    (display "*")
)
(newline)
(newline)
```
{{< /note >}}
