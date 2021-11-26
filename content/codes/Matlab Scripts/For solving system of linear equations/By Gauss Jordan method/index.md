---
title: Gauss Jordan method
date: 2021-08-23
menu:
  codes:
    name: Gauss Jordan method
    identifier: gauss-jordan
    parent: solving-SLE
    weight: 2
---
{{< note title="Script for Gauss Jordan method" >}}

This matlab script can solve a system of linear equations by Gauss Jordan method with partial pivoting. Co-efficeint matrix and contant vector are be prompted for user input. 
Fun fact is, this script can show the calculation steps.
</br>
```matlab

%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%%
%   Title:  Gauss Jordan with partial pivoting                      %
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

solByGJPP = gaussJordanPartPiv(A, b)

function [x] = gaussJordanPartPiv(A, b)
    global showSteps;
    [m, n] = size(A);
    if m~=n
        error("'A' must be a square matrix.");
    else
        aug = [A b];
        
        if showSteps == 1
            fprintf("Gauss Jordan with partial pivoting.\nAugmented Matrix:\n");
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
                aug(i,k:n+1) =  aug(i,k:n+1) - factor*aug(k,k:n+1);
            end
            
            %display augmented matrix after each step
            if showSteps == 1
                fprintf("\n");
                disp(aug);
            end
        end
        
        %elimination from upper triangular matrix
        if showSteps == 1
            fprintf("Elimination from upper triangular matrix:\n\n");
        end
        for k = n:-1:2
            for i = k-1:-1:1
                if showSteps == 1
                    fprintf("Row%d = Row%d - (%0.4f)/(%0.4f) * Row%d\n", i, i, aug(i,k), aug(k,k), k);
                end
                factor = aug(i,k)/aug(k,k);
                aug(i,n+1:-1:i) = aug(i,n+1:-1:i) - factor*aug(k,n+1:-1:i);
            end
            if showSteps == 1
                fprintf("\n");
                disp(aug)
            end
        end
        
        %converting to Identity Matrix
        for i = 1:n
            aug(i,:) = aug(i,:)/aug(i,i);
        end
        if showSteps == 1
            fprintf("\nFinal form of the augmented matrix:\n\n");
            disp(aug);
            fprintf("\n");
        end        
    end
    x = aug(:,n+1);
end

```
{{< /note >}}
