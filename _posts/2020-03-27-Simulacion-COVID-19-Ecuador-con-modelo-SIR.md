---
layout: post
title:  "Simuacion COVID-19 Ecuador modelo SIR"
date:   25-03-2020
categories: simulaciones
mathjax: true
---
Dejenme introducirles el modesto modelo SIR:

$$
\begin{align}
&\frac{dS}{dt} = - \beta SI \label{eq:caca} \\[5pt]
&\frac{dI}{dt} = \beta SI - \gamma I \label{eq:caca1} \\[5pt]
&\frac{dR}{dt} = \gamma I \label{eq:caca2}
\end{align}$$
con las condiciones iniciales $S(0) = S_0 >0, I(0) = I_0 >0$, $R(0) = 0$. Fijate que $S(t) + I(t) +  R(t) =  S_0 + I_0 = N$, ya que la poblacion, por asuncion, es constante.
