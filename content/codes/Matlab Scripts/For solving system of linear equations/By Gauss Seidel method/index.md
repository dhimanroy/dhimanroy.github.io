---
title: Gauss Seidel method
date: 2021-08-23
menu:
  codes:
    name: Gauss Seidel method
    identifier: gauss-seidel
    parent: solving-SLE
    weight: 4
---
{{< note title="Script for Gauss Seidel method" >}}
This matlab script can solve a system of linear equations by Gauss Seidel method, an iterative method. Co-efficeint matrix, contant vector and tolerence are be prompted for user input. 
<br/>
```matlab

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%   Title:  Gauss Seidel method                                     %
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

solByGaussSiedel = gaussSeidel(A, b)

function [x] = gaussSeidel(A, b)
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
        error("Partial Pivoting does not make the coefficient matrix a diagonally dominant matrix.")
    else
        disp("Partial Pivoting makes the coefficient matrix a diagonally dominant matrix.")
    end
    
    x = xIni;
    xOld = x;
    
    for k = 1:100
        err = 0;
        for i = 1:n
            x(i) = (b(i)-A(i,[1:i-1,i+1:n])*x([1:i-1,i+1:n]))/A(i,i);
            err = err + abs((x(i)-xOld(i))/x(i));
            xOld(i) = x(i);
        end
        if showSteps == 1
            fprintf("Iteration %d\n",k);
            disp(x);
            disp(err);
        end
        
        if err < eLimit
            break
        end
    end
end

```
{{< /note >}}
