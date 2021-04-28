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

\begin{bmatrix}
Y\cr
U\cr
V
\end{bmatrix}=
\begin{bmatrix}
R\cr
G\cr
B
\end{bmatrix}

## YCbCr
輝度($Y$)，色差($C_b$，$C_r$)による表色系．直交座標．RGB表色系からの変換は次式で表される．

$Y = R + G + B$
## HSV
## CIE L*a*b*
----
[チートシート目次へ戻る](./index.md)
