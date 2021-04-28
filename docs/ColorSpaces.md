<script type="text/javascript" async src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.7/MathJax.js?config=TeX-MML-AM_CHTML">
</script>
<script type="text/x-mathjax-config">
 MathJax.Hub.Config({
 tex2jax: {
 inlineMath: [['$', '$'] ],
 displayMath: [ ['$$','$$'], ["\\[","\\]"] ]
 }
 });
</script>

# 表色系（色空間）
## RGB
赤($R$)，緑($G$)，青($B$)を直交座標の軸にした表色系．

## YUV
輝度($Y$)，色差($U$，$V$)による表色系．直交座標．RGB表色系からの変換は次式で表される．

\begin{align}
\begin{bmatrix}
Y\cr
U\cr
V
\end{bmatrix}
=
\begin{bmatrix}
k_R & 1-k_R-k_B & k_B \cr
\frac{-0.5k_R}{1-k_B} & \frac{-0.5(1-k_R-k_B)}{1-k_B} & 0.5 \cr
0.5 & \frac{-0.5(1-k_R-k_B)}{1-k_R} & \frac{-0.5k_B}{1-k_R} 
\end{bmatrix}
\begin{bmatrix}
R\cr
G\cr
B
\end{bmatrix}
\end{align}

なお，YCbCr表色系からRGB表色系への変換式は，上記の逆行列を求めることで得られる．

\begin{align}
\begin{bmatrix}
R\cr
G\cr
B
\end{bmatrix}
=
\begin{bmatrix}
1 & 0 & 2(1-k_R) \cr
1 & \frac{-2k_B(1-k_B)}{1-k_R-k_B} & \frac{-2k_R(1-k_R)}{1-k_R-k_B} \cr
1 & 2(1-k_B) & 0
\end{bmatrix}
\begin{bmatrix}
Y\cr
U\cr
V
\end{bmatrix}
\end{align}

## YCbCr (ITU-R BT.601方式)
輝度($Y$)，色差($C_b$，$C_r$)による表色系．直交座標．RGB表色系からの変換式は
YUVにおける変換式に$k_R=0.299$，$k_B=0.114$を代入することで得られる．

\begin{align}
\begin{bmatrix}
Y\cr
C_b\cr
C_r
\end{bmatrix}
=
\begin{bmatrix}
0.299 & 0.587 & 0.114 \cr
-0.169 & -0.331 & 0.5 \cr
0.5 & -0.419 & -0.081
\end{bmatrix}
\begin{bmatrix}
R\cr
G\cr
B
\end{bmatrix}
\end{align}

\begin{align}
\begin{bmatrix}
R\cr
G\cr
B
\end{bmatrix}
=
\begin{bmatrix}
1 & 0 & 1.402 \cr
1 & -0.344 & -0.714 \cr
1 & 1.772 & 0
\end{bmatrix}
\begin{bmatrix}
Y\cr
C_b\cr
C_r
\end{bmatrix}
\end{align}

## YPbPr (ITU-R BT.709方式)
輝度($Y$)，色差($P_b$，$P_r$)による表色系．直交座標．RGB表色系からの変換式は
YUVにおける変換式に$k_R=0.2126$，$k_B=0.0722$を代入することで得られる．

\begin{align}
\begin{bmatrix}
Y\cr
P_b\cr
P_r
\end{bmatrix}
=
\begin{bmatrix}
0.2126 & 0.7152 & 0.0722 \cr
-0.1146 & -0.3854 & 0.5 \cr
0.5 & -0.4542 & -0.0458
\end{bmatrix}
\begin{bmatrix}
R\cr
G\cr
B
\end{bmatrix}
\end{align}

\begin{align}
\begin{bmatrix}
R\cr
G\cr
B
\end{bmatrix}
=
\begin{bmatrix}
1 & 0 & 1.5748 \cr
1 & -0.1873 & -0.4681 \cr
1 & 1.8556 & 0
\end{bmatrix}
\begin{bmatrix}
Y\cr
P_b\cr
P_r
\end{bmatrix}
\end{align}

## HSV
## CIE $L^*a^*b^*$
----
[チートシート目次へ戻る](./index.md)
