---
title: Jacobi method
date: 2021-08-23
menu:
  codes:
    name: Jacobi method
    identifier: jacobi
    parent: solving-SLE
    weight: 53
---
{{< note title="Script for Jacobi method" >}}
Another iterative method for solving a system of linear equations.
<br/>
```matlab

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%   Title:  Jacobi method                                           %
%   Author: Dhiman Roy, Dept. of ME (BUET)                          %
%   Date:   August 23, 2021                                         %
%   Licensed under Creative Commons                                 %
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

clear; 
clc;
global showSteps;
showSteps = false; %true shows calculation steps

A = input('Please Enter the Co-efficient Matrix, A:\n' ); 
b = input('Please Enter the Constants(Must be a column vector, b:\n'); 

solByJacobi = jacobi(A, b)

function [x] = jacobi(A, b)
    global showSteps;
    xIni = input("Enter initial guesses (Must be a column vector:\n"); 
    eLimit = input("Enter tolerance:\n");
    [m,n] = size(A);
    for k = 1: n-1
        for p = k+1:n
            if abs(A(k,k) < A(p,k))
                b([k p]) = b([p k]);
                A([k p],:) = A([p k],:);
            end
        end
    end
    check = ones(n,1);
    for i = 1:n
        if 2*abs(A(i,i)) < sum(abs(A(i,:))) 
            check(i) = 0;
        end
    end
    if all(check) ~= 1
        error("Partial pivoting does not make the coefficient matrix a diagonally dominant matrix.")
    else
        disp("Partial pivoting makes the coefficient matrix a diagonally dominant matrix.")
    end
    
    xOld = xIni;
    xNew = zeros(n,1);
    for k = 1:100
        err = 0;
        for i = 1:n
            xNew(i) = b(i)/A(i,i)-A(i,[1:i-1,i+1:n])*(xOld([1:i-1,i+1:n])/A(i,i));
            err = err + abs((xNew(i)-xOld(i))/xNew(i));
        end
        if showSteps == 1
            fprintf("Iteration %d\n",k);
            disp(xNew);
            abs((xNew-xOld)/xNew);
        end
        
        
        if err < eLimit
            break;
        end
        xOld = xNew;
    end
    x = xNew;
end
```
{{< /note >}}
