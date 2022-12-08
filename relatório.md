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
\begin{bmatrix} x_1
\\ x_2
\\ x_3
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
\begin{bmatrix} x_1
\\ x_2
\\ x_3
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

Para os valores numéricos do item anterior, temos:
$$
G(s) = \frac{126.6}{s^3+100s^2-108.1s-3924\cdot10^4}
$$


## Projeto de Controle a Tempo Contínuo

### Projeto do controlador

\begin{center}
    \includegraphics[scale=0.5]{malha-fechada}
\end{center}

Com base na estrutura de controle em malha fechada da figura acima, foi feito o projeto do controlador $C(s)$ de forma a estabilizar a planta $G(s)$. O controlador deve atender requisitos de projetos que são discutidos a seguir.

- Erro nulo para entrada degrau

    Analisando a planta $G(s)$ notamos que ela não possui polos na origem, logo ela é dita do tipo 0. Para que o sistema em malha fechada possua erro nulo para entrada degrau ele precisa ser do tipo 1, portanto o controlador $C(s)$ deve ter pelo menos um polo na origem.

- Tempo de estabilização menor que 4 segundos

    O tempo de estabilização define um limitante de quão próximo os polos dominantes do sistema em malha fechada devem estar do eixo imaginário. Consideramos $\epsilon_t = 2\%$:

$$t_e = \frac{-ln(\epsilon_t)}{\xi \omega_n} < 4s $$
$$\frac{-ln(0.02)}{\xi\omega_n} < 4s$$
$$\xi\omega_n > 1$$

- Margem de fase MF > 30º
  
    Pela aproximação $MF \approx 100\xi$ temos que $\xi>0.3$

- Valor de pico da posição $\text{max}_{t\ge0} y(t) \le 0.04$ para entrada degrau de amplitude $0.02$
  
$$\text{max} \;\; y(t) = 0.02 + e^{\frac{-\xi\pi}{\sqrt{1-\xi^2}}} \le 0.04$$
$$\xi \ge 0.778$$

- Esforço de controle $|u| \le 200V$ para entrada degrau de amplitude $0.02$

    Este critério será analisado *a posteriori* para ver se o controlador não gasta muito para estabilizar a planta. Com base na figura do sistema em malha fechada temos
$$\frac{\hat{u}(s)}{\hat{r}(s)} = \frac{C(s)}{1+C(s)G(s)}$$

Com este parâmetros definidos, utilizamos o método do lugar das raízes para projetar o controlador:

- Inicialmente, analisamos o lugar das raízes de $G(s)$ e notamos que ele é sempre instável.
\begin{center}
    \includegraphics[scale=0.5]{rootsGs}
\end{center}
- Depois, aplicamos um controlador integrador com polo na origem para atender o requisito de erro nulo para entrada degrau e notamos que seus polos dominantes determinarão sempre um sistema instável.
\begin{center}
    \includegraphics[scale=0.5]{rootsGss}
\end{center}
- Com isso, adicionamos dois zeros em $s=-8$ para que seja possível ter os polos dominantes no semiplano esquerdo. Determinamos a região $\Omega$ com os requisitos dados.
\begin{center}
    \includegraphics[scale=0.5]{regiao}
\end{center}
- Para que o controlador fosse implementável, adicionamos um polo $(\tau s+1)$ com $\tau = 0.008$ suficientemente pequeno.
- Com a ferramenta `sisotool` do MATLAB determinamos o ganho $\kappa = 78$ que alocasse os polos dominantes do sistema em malha fechada na região $\Omega$ e que cumprisse todos requisitos.

Deste modo, o controlador resultante do projeto é

$$C(s) = 78\frac{(s+8)^2}{s(0.008s+1)}$$

A seguir está a resposta do sistema em malha fechada ao degrau de amplitude $0.02$. Note que todos requisitos foram atendidos.

\begin{center}
    \includegraphics[scale=0.8]{resposta}
    \includegraphics[scale=0.8]{esforço}
\end{center}

### Análise do controlador no sistema não-linear

Com o controlador $C(s)$ determinado anteriormente, tentamos aplicá-lo no sistema não-linear do levitador magnético no intuito de analisar os efeitos da linearização e a eficiência deste método.

Com auxílio do Simulink, montamos o sistema dado pelas equações de estado do modelo não linear e conectamos $C(s)$ na entrada de controle, implementamos o *feedback* da malha fechada e inserimos uma entrada degrau de mesma amplitude $0.02$.

\begin{center}
    \includegraphics[scale=0.4]{simulink}
\end{center}

A simulação, no entanto, não deu certo, com a resposta $y(t)$ tendendo para o infinito. Este resultado se deve provavelmente a algum erro no processo de linearização que não foi visto.

## Projeto de Controle a Tempo Contínuo

### Equivalente discreto

Com base no controlador projetado a tempo contínuo, determinamos seus equivalentes discretos com período de amostragem $T = 0.001s$ por meio de diferentes métodos utilizando o comando `c2d` do MATLAB:

- Segurador de Orgem Zero
$$C_S(z)=\frac{9750z^2-19350z+9604}{z^2-1.882z+0.8825}$$
- Tustin
$$C_T(z)=\frac{9750z^2-18350z+9103}{z^2-1.882z+0.8824}$$
- Mapeamento de Polos e Zeros
$$C_M(z)=\frac{9239z^2-18330z+9092}{z^2-1.882z+0.8825}$$

A partir destes resultados, aplicamos os controladores discretizados no sistema em malha fechada e observamos a resposta ao degrau de amplitude $0.01$ e o esforço de controle de cada um.

\begin{center}
    \includegraphics[scale=0.35]{resposta-discreto}
    \includegraphics[scale=0.35]{esforço-discreto}
\end{center}

Analisando os gráficos, não é possível notar muita diferença entre os controladores, principalmente entre o segurador de ordem zero e o Tustin.

### Projeto direto

Primeiramente, discretizamos a planta pelo método segurador de ordem zero com $T = 0.01s$, o que resultou em 
$$G(z) = \frac{1.673\cdot10^{-5}z^2+5.137\cdot10^{-5}z+1.016\cdot10^{-5}}{z^3-2.389z^2-1.732z-0.3677}$$
Com isso, utilizamos a ferramenta `sisotool` para projetar o controldor $C(z)$. A figura a seguir mostra o lugar das raízes do sistema em malha fechada já com o controlador implementado e a região de projeto com requisitos de $t_e < 4s$ e $\xi > 0.8$. O controlador projetado que estabiliza o sistema e atende os requisitos citados é do tipo derivativo e possui um zero em $z=0.74$. O ajuste do ganho foi feito e resultou no controlador
$$C(z) = 1279(z-0.74)$$

\begin{center}
    \includegraphics[scale=0.4]{root-discrete}
\end{center}