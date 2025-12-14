---
layout: post
title:  "Development Journal for universal washing machine damper"
date:   2025-12-13 18:30:00 +0200
categories: jekyll update
---

# Development Journal
# Universal Washing Machine Damper

## 1 Introduction 
This is the development journal for an open source hardware washing machine damper. The damper is the most important part for long mechanical life of a washing machine. ...

## 2 General technical facts

what are typical damper types? which one fail sooner, which ones last? damper parameters? loads/stresses/frequencies/ damping-power /thermal losses/ differential equations that describe the system of mass/spring coeff/damper —> forces —-> design

+ length: tbd.
+ mounting holes: 8.0 mm for fitting screw
 
## 3 fundamental differential equations for mass-spring-damper-sytsem

The following [GNU Octave](https://octave.org/download.html "https://octave.org/download.html") script calculates and plots the amplitude of a damped mass-spring oscillator depending on the excitation frequency.


```
% spring_dampe_mass_amplitude.m
% Calculates and plots the amplitude of a damped
% spring–mass oscillator as a function of the excitation frequency.

clear all; close all; clc;

% ---- Parameters -------------------------------------------------
m  = 1.0;        % Mass / kg
k  = 100.0;      % Spring constant / N/m
c  = 1.0;        % Damping constant / N·s/m (0 = undamped)
F0 = 1.0;        % Amplitude of the excitation force / N

% Natural angular frequency and natural frequency
omega0 = sqrt(k/m);           % / rad/s
f0     = omega0/(2*pi);       % / Hz

% Frequency axis around the natural frequency
f_min = 0.0 * f0;             % Lower limit / Hz
f_max = 3.0 * f0;             % Upper limit / Hz
N     = 1000;                 % Number of support points

f     = linspace(f_min, f_max, N);  % Frequency [Hz]
omega = 2*pi*f;                     % Angular frequency [rad/s]

% ---- Amplitude calculation --------------------------------------
% A(omega) = F0 / sqrt((k - m*omega^2)^2 + (c*omega)^2)
num  = F0;
den  = sqrt( (k - m.*omega.^2).^2 + (c.*omega).^2 );
A    = num ./ den;

% ---- Spring force calculation -----------------------------------
% Peak spring force magnitude: |F_spring| = k * A (from F_s = -k x)
F_spring = k .* A;  % / N

% ---- Plot ------------------------------------------------------
figure;
plot(f, A, 'LineWidth', 2, 'DisplayName', 'Amplitude');
hold on;
plot(f, F_spring/1000, 'LineWidth', 2, 'DisplayName', 'Spring Force /kN');  % Scaled for visibility
grid on;
xlabel('Frequency f /Hz');
ylabel('Amplitude A /m , Force / kN');
title('Amplitude and Spring Force of Damped Spring-Mass Oscillator');
legend('Location', 'best');

% Enlarge font size for all axes text (title, labels, ticks, legend)
set(gca, 'FontSize', 16);  % Adjust 16 to your preferred size (e.g., 20 for even larger)

% Mark the natural frequency
y_eig = interp1(f, A, f0);     % Amplitude at natural frequency
plot(f0, y_eig, 'ro', 'MarkerSize', 8, 'LineWidth', 2);
text(f0, y_eig, sprintf('  f_0 = %.2f Hz', f0), ...
     'VerticalAlignment', 'bottom');

drawnow;
% pause;    % Optionally enable if you want the window to remain open

```


## 2Do / Next Steps

0. Documatation of physical and mathematical fundamentales to calculate load / stresses / amplidudes / optimum values by equations, figures, etc. All 'scientific' knowledge should be gathered. It's the base for opimisation by calculations and not trying ;)
   
2. Determination of typical values for
- spring constants
- damper constants
- massrange of empty and fully loaded washing drum
in washing machines.

2. Note values in this document
3. calculation of forces and amplitudes, maximum allowable amplitudes / imbalance of loundry
4. Investigation / List of available oil damper
5. concept of frictionless, wearless, electromagnetic damper by Eddy Current or by generator coil
6. estimation of unbalance force

...





