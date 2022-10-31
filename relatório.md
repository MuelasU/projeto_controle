---
header-includes:
    - \usepackage{graphicx}
    - \graphicspath{ {./images/} }
...

# Projeto Controle de um Levitador Magnético

### ES710 - Controle de sistemas mecânicos

#### Alunos
- Gabriel Muelas Urano, 234944
- Luigi De Salvo Miotti, 221040

## Introdução

Neste projeto analisamos o sistema de um levitador magnético. O sistema consiste de uma bola de material magnético com massa $m$ suspensa por um eletroímã, cuja corrente é controlada via realimentação, utilizando a medida da posição da bola $y$, que por sua vez é obtida por um sensor óptico. A posição vertical $y \ge 0$ é medida a partir de um ponto de referencia em que $y = 0$ quando a bola está encostada no eletroímã. Nesta figura, $\kappa$ é o coeficiente de atrito viscoso, $g$ é a aceleração da gravidade, $F(y,i)$ é a força gerada pelo eletroímã e $i$ é a corrente elétrica associada.

\begin{center}
    \includegraphics[scale=0.5]{sistema}
\end{center}

A indutância do eletroímã pode ser modelada por
$$L(y) = L_1 + \frac{L_0}{1+y/a}$$

A força é dada por
$$F(y, i) = -\frac{L_0i^2}{2a(1+y/a)}$$

Considerando que o sistema é alimentado por uma fonte de tensão $v$ temos que $v = \dot{\phi}+Ri$ com $\phi = L(y)i\;$ sendo o fluxo magnético.

## Análise do Sistema a Tempo Contínuo

### 1. Modelo não-linear

A análise do sistema começa pela modelagem do sistema, que foi dividido em sistema elétrico e dinâmico, o que resultou em duas equações de variáveis $y$, $i$ e suas derivadas no tempo.

- Sistema Elétrico

\begin{center}
    \includegraphics[scale=0.5]{circuit}
\end{center}

$$v = Ri + \dot\phi$$
$$v = Ri + (L(y)'\cdot i + L(y)\cdot i')$$
$$v = Ri + \left( -aL_0\frac{y'}{(a+y)^2}i + L_1i' + \frac{aL_0}{a+y}i'\right)$$
$$v = \left[ R - aL_0\frac{y'}{(a+y)^2} \right]i + \left[ L_1 + \frac{aL_0}{a+y}\right]i'$$

- Sistema Dinâmico

\begin{center}
    \includegraphics[scale=0.5]{ball}
\end{center}

$$my'' = F(y, i) + mg - \kappa y'$$
$$my'' + \frac{L_0i^2}{2a(1+y/a)^2} - mg + \kappa y' = 0$$

### 2. Representação em espaço de estados

Adotamos as seguintes variáveis de estado $\xi_1=y,\; \xi_2=y',\; \xi_3=i$, entrada de controle $u_N=-v$ e saída $z_N=\xi_1$.

Reescrevendo as equações do sistema não-linear, obtemos
$$v - \left[ R - aL_0\frac{y'}{(a+y)^2} \right]i = \left[ L_1 + \frac{aL_0}{a+y}\right]i'$$
$$i' = \frac{v - \left[ R - aL_0\frac{y'}{(a+y)^2} \right]i}{ L_1 + \frac{aL_0}{a+y}}$$


e
$$my'' + \frac{L_0i^2}{2a(1+y/a)^2} - mg + \kappa y' = 0$$
$$y'' = g - \frac{\kappa}{m}y' - \frac{aL_0i^2}{2m(a+y)^2}$$

Portanto, a representação em espaço de de estados do sistema é dada por

$$\xi_1' = \xi_2$$
$$\xi_2' = g - \frac{\kappa}{m}\xi_2 - \frac{aL_0\xi_3^2}{2m(a+\xi_1)^2}$$
$$\xi_3' = \frac{-u_N - \left[ R - aL_0\frac{\xi_2}{(a+\xi_1)^2} \right]\xi_3}{ L_1 + \frac{aL_0}{a+\xi_1}}$$

### 3. Posição de equilibrio

Desejamos manter a bola em equilíbrio na posição $y_e=0$, isto é, encostada no eletroímã. Para isso adotamos que $y^{(n)}=0$, com $n$ sendo a $n$-ésima derivada no tempo. Aplicando estas condições nas equações do modelo não-linear obtemos:

$$0 + \frac{L_0i_e^2}{2a(1+0)^2} - mg + 0 = 0$$
$$\frac{L_0i_e^2}{2a} - mg = 0$$
$$i_e = \sqrt{\frac{2amg}{L_0}} $$
e
$$v_e = \left[ R - 0 \right]i_e + \left[ L_1 + \frac{aL_0}{a+0}\right]i'_e$$
$$v_e = Ri_e + L_1i'_e + L_0i'_e$$

Como em regime permanente a corrente fica constante, então $i'_e = 0$ e, portanto, temos

$$v_e = Ri_e$$

Concluímos que para manter a bola em equilíbrio na posição $y_e = 0$ precisamos que $i_e = \sqrt{\frac{2amg}{L_0}}$ e $v_e = Ri_e$

### 4. Modelo linearizado

A linearização das equações em torno do ponto de equilíbrio $(y_e, \dot{y}_e, i_e)$ é dada pela expansão de Taylor em primeira ordem

$$f(y, \dot{y}, i) = f(y_e, \dot{y}_e, i_e) + \frac{\partial f(y_e, \dot{y}_e, i_e)}{\partial y}(y-y_e) + \frac{\partial f(y_e, \dot{y}_e, i_e)}{\partial \dot{y}}(\dot{y}-\dot{y}_e) + \frac{\partial f(y_e, \dot{y}_e, i_e)}{\partial i}(i-i_e) $$

O resultado desta expansão para as equações do sistema não-linear é dado pela equações abaixo que configuram o sistema linearizado do problema.

$$v - Ri + \frac{L_0i_e}{a}y'-(L_0+L_1)i'=0$$
$$y''- g + \frac{k}{m}y'+ \frac{L_0i_e}{ma}\left[\frac{i_e}{2} + (i-i_e) - \frac{i_e}{a}(y-y_e)\right]=0$$

### 5. Representação em espaço de estados do modelo linearizado

Adotamos as variáveis de estado $x_1=y-y_e, x_2=\dot{y}$ e $x_3 = i-i_e$, entrada $u=v_e-v$ e saída $z=x_1$. Para colocar na forma matricial, rearranjamos a expansão de Taylor para as novas variáveis.

$$f(y, \dot{y}, i) - f(y_e, \dot{y}_e, i_e) = \frac{\partial f(y_e, \dot{y}_e, i_e)}{\partial y}(y-y_e) + \frac{\partial f(y_e, \dot{y}_e, i_e)}{\partial \dot{y}}(\dot{y}-\dot{y}_e) + \frac{\partial f(y_e, \dot{y}_e, i_e)}{\partial i}(i-i_e) $$

$$f(x_1, x_2, x_3) = \frac{\partial f(y_e, \dot{y}_e, i_e)}{\partial y}x_1 + \frac{\partial f(y_e, \dot{y}_e, i_e)}{\partial \dot{y}}x_2 + \frac{\partial f(y_e, \dot{y}_e, i_e)}{\partial i}x_3 $$

Com isto, obtemos as seguintes equações

$$\dot{x_3} = \frac{\tfrac{L_0 i_e}{a}x_2-u-Rx_3}{L_0+L_1}$$

$$\dot{x_2} = \frac{L_0 i_e^2}{ma^2}x_1 - \frac{k}{m}x_2 - \frac{L_o i_e}{ma}x_3$$

Portanto, na forma matricial temos

$$\begin{bmatrix} \dot{x_1}
\\ \dot{x_2}
\\ \dot{x_3}
\end{bmatrix}
=
\begin{bmatrix}
0 & 1 & 0\\ 
\frac{L_0 i_e^2}{ma^2} & -\frac{k}{m} & -\frac{L_o i_e}{ma}\\ 
0 & \frac{L_0}{L_0+L_1}\frac{i_e}{a} & -\frac{R}{L_0+L_1}
\end{bmatrix}
\begin{bmatrix} \dot{x_1}
\\ \dot{x_2}
\\ \dot{x_3}
\end{bmatrix}
+
\begin{bmatrix} 0
\\ 0
\\ -\frac{1}{L_0+L_1}
\end{bmatrix}
u$$

$$
z=
\begin{bmatrix}
    1 & 0 & 0
\end{bmatrix}
\begin{bmatrix} \dot{x_1}
\\ \dot{x_2}
\\ \dot{x_3}
\end{bmatrix}
+ [0]u
$$

### 6. Estabilidade

Avaliando computacionalmente a matriz A com os valores $m=0.25$, $\kappa = 10^{-3}$, $L_0 = 0.05$, $L_1 = 0.02$, $g=9.81$, $a=0.05$ e $R=7$, temos:

$$
A = \begin{bmatrix}
0 & 1 & 0\\ 
392.4 & -0.04 & -6.2642\sqrt{2}\\ 
0 & 22.372\sqrt{2} & -100
\end{bmatrix}
$$

Conforme consultado em “Process Control: Modeling, Design and Simulation” (B. Wayne Bequette), para que um sistema no espaço de estados seja assintoticamente estável, todos os autovalores da matriz A precisam ter parte real negativa. Dessa forma, calculamos os autovalores da matriz com o comando `eig` e  obtemos os seguintes resultados:
$$\lambda _1 = 18.64427\:\:\:
\lambda _2 = -21.7014\:\:\:
\lambda _3 = -96.9828$$

Nota-se que um dos autovalores é positivo, o que é suficiente para concluir que o sistema é instável.

### 7. Função de transferência

Para calcular a função de transferência $G(s) = \frac{\hat{z}(s)}{\hat{u}(s)}$ a partir da equação do espaço de estados, podemos usar a fórmula
$$G = C(sI-A)^{-1}B+D$$
Com o auxílio do computador, obtemos a função de transferência:
$$
G(s) =-\frac{- L_{0}^{2} a i_{e} - L_{0} L_{1} a i_{e}}{\left(L_{0} + L_{1}\right) \left(- L_{0} L_{1} i_{e}^{2} s - L_{0} R i_{e}^{2} + L_{0} a^{2} k s^{2} + L_{0} a^{2} m s^{3} + L_{1} a^{2} k s^{2} + L_{1} a^{2} m s^{3} + R a^{2} k s + R a^{2} m s^{2}\right)}
$$
