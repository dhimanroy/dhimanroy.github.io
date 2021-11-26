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
(define x-cor (list 0.039	0.042	0.0457	0.048	0.05	0.053	0.056	0.059	0.062	0.065	0.068	0.071	0.074	0.077	0.08	0.085	0.09	0.095	0.1	0.11	0.12	0.13	0.14	0.15	0.035	0.042	0.0457	0.048	0.05	0.053	0.056	0.0589	0.062	0.065	0.068	0.071	0.074	0.077	0.08	0.083	0.086	0.089	0.092	0.095	0.1	0.11	0.12	0.13	0.14	0.15	0.0457	0.048	0.05	0.053	0.056	0.059	0.062	0.065	0.068	0.071	0.074	0.077	0.08	0.085	0.09	0.0457	0.048	0.05	0.053	0.056	0.059	0.062	0.065	0.068	0.071	0.074	0.077	0.08	0.085	0.09	0.0457	0.048	0.05	0.053	0.056	0.059	0.062	0.065	0.068	0.071	0.074	0.077	0.08	0.085	0.09	0.071	0.074	0.077	0.08	0.083	0.086	0.089	0.092	0.095	0.1	0.071	0.074	0.077	0.08	0.083	0.086	0.089	0.092	0.095	0.1	0.071	0.074	0.077	0.08	0.083	0.086	0.089	0.092	0.095	0.1	0.0457	0.056	0.056	0.068	0.068	0.071	0.071	0.074	0.074	0.077	0.077	0.095	0.095	0.1	0.1
))
(define y-cor (list -0.0154	-0.0167	-0.018	-0.018	-0.018	-0.018	-0.018	-0.018	-0.018	-0.018	-0.018	-0.018	-0.018	-0.018	-0.018	-0.018	-0.018	-0.018	-0.018	-0.018	-0.018	-0.0176	-0.0167	-0.0159	-0.029	-0.03	-0.0307	-0.031	-0.0315	-0.032	-0.0324	-0.033	-0.033	-0.033	-0.033	-0.033	-0.033	-0.033	-0.033	-0.033	-0.033	-0.033	-0.033	-0.033	-0.033	-0.033	-0.033	-0.033	-0.033	-0.033	-0.019	-0.019	-0.019	-0.019	-0.019	-0.019	-0.019	-0.019	-0.019	-0.019	-0.019	-0.019	-0.019	-0.019	-0.019	-0.02	-0.02	-0.02	-0.02	-0.02	-0.02	-0.02	-0.02	-0.02	-0.02	-0.02	-0.02	-0.02	-0.02	-0.02	-0.021	-0.021	-0.021	-0.021	-0.021	-0.021	-0.021	-0.021	-0.021	-0.021	-0.021	-0.021	-0.021	-0.021	-0.021	-0.032	-0.032	-0.032	-0.032	-0.032	-0.032	-0.032	-0.032	-0.032	-0.032	-0.031	-0.031	-0.031	-0.031	-0.031	-0.031	-0.031	-0.031	-0.031	-0.031	-0.03	-0.03	-0.03	-0.03	-0.03	-0.03	-0.03	-0.03	-0.03	-0.03	-0.024	-0.024	-0.026	-0.024	-0.026	-0.024	-0.026	-0.024	-0.026	-0.024	-0.026	-0.024	-0.026	-0.024	-0.026
))
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
