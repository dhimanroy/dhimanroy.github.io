---
title: Gauss elimination method
date: 2021-08-23
menu:
  codes:
    name: Gauss Elimination method
    identifier: gauss-elimination
    parent: solving-SLE
    weight: 1
---
{{< note title="Script for Gauss Elimination method" >}}

This matlab script can solve a system of linear equations by Gauss elimination method with partial pivoting.
Another fun fact is, this script can show the calculation steps. So, this script saves a lot of effort while
preparing assignments.
</br>
```matlab

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%   Title:  Gauss Elimination with partial pivoting                 %
%   Author: Dhiman Roy, Dept. of ME (BUET)                          %
%   Date:   August 23, 2021                                         %
%   Licensed under Creative Commons                                 %
%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%

clear; 
clc;
global showSteps;
showSteps = true; %can be true or false. true shows calculation steps

A = input('Please Enter the Co-efficient Matrix, A:\n' ); 
b = input('Please Enter the Constants(Must be a column vector, b:\n');

solByGEPP = gaussElimPartPiv(A, b)

function [x] = gaussElimPartPiv(A, b)
    global showSteps;
    [m, n] = size(A);
    if m~=n
        error("'A' must be a square matrix.");
    else
        aug = [A b];
        if showSteps == 1
            fprintf("Gauss elimination with partial pivoting.\nAugmented Matrix:\n");
            disp(aug);
            fprintf("\n");
        end
        
        for k = 1: n-1
            %partial pivoting
            for p = k+1:n
                if abs(A(k,k) < A(p,k))
                    aug([k p],:) = aug([p k],:);
                end
            end
            %forward elimination
            for i = k+1: n
                if showSteps == 1
                    fprintf("Row%d = Row%d - (%0.4f)/(%0.4f) * Row%d\n", i, i, aug(i,k), aug(k,k), k);
                end
                factor = aug(i,k)/aug(k,k);
                aug(i,k:n+1) =  aug(i,k:n+1)-factor*aug(k,k:n+1);
            end
            
            %display augmented matrix after each step
            if showSteps == 1
                fprintf("\n");
                disp(aug)
                fprintf("\n");
            end
        end
        %back substitution
        x = zeros(n,1);
        x(n) = aug(n,n+1)/aug(n,n);
        for i = n-1:-1:1
           x(i) = (aug(i,n+1)-aug(i,i+1:n)*x(i+1:n))/aug(i,i);
        end
    end
end

```