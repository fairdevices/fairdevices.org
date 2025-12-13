---
layout: post
title:  "Development Journal for universal washing machine damper"
date:   2025-12-08 18:30:00 +0200
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

The following [Octave Download](https://octave.org/download.html "GNU Octave") script calculates and plots the amplitude of a damped mass-spring oscillator depending on the excitation frequency.

{% raw %}{% highlight octave %}
% feder_masse_amplitude.m
% Berechnet und plottet die Amplitude eines gedämpften
% Feder-Masse-Schwingers in Abhängigkeit von der Anregungsfrequenz.

clear all; close all; clc;

% ---- Parameter -------------------------------------------------
m  = 1.0;        % Masse [kg]
k  = 100.0;      % Federsteifigkeit [N/m]
c  = 1.0;        % Dämpfungskonstante [N·s/m] (0 = ungedämpft)
F0 = 1.0;        % Amplitude der Erregerkraft [N]

% Eigenkreisfrequenz und Eigenfrequenz
omega0 = sqrt(k/m);           % [rad/s]
f0     = omega0/(2*pi);       % [Hz]

% Frequenzachse um die Eigenfrequenz herum
f_min = 0.0 * f0;             % untere Grenze [Hz]
f_max = 3.0 * f0;             % obere Grenze [Hz]
N     = 1000;                 % Anzahl Stützstellen

f     = linspace(f_min, f_max, N);  % Frequenz [Hz]
omega = 2*pi*f;                     % Kreisfrequenz [rad/s]

% ---- Amplitudenberechnung --------------------------------------
% A(omega) = F0 / sqrt((k - m*omega^2)^2 + (c*omega)^2)
num  = F0;
den  = sqrt( (k - m.*omega.^2).^2 + (c.*omega).^2 );
A    = num ./ den;

% ---- Plot ------------------------------------------------------
figure;
plot(f, A, 'LineWidth', 2);
grid on;
xlabel('Frequenz f [Hz]');
ylabel('Amplitude A [m]');
title('Amplitude eines gedämpften Feder-Masse-Schwingers');

% Markiere Eigenfrequenz
hold on;
y_eig = interp1(f, A, f0);     % Amplitude bei Eigenfrequenz
plot(f0, y_eig, 'ro', 'MarkerSize', 8, 'LineWidth', 2);
text(f0, y_eig, sprintf('  f_0 = %.2f Hz', f0), ...
     'VerticalAlignment', 'bottom');

% Ausgabe sicherstellen, falls Skript nicht interaktiv läuft
drawnow;
% pause;    % optional aktivieren, wenn das Fenster offen bleiben soll
{% endhighlight %}{% endraw %}

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




