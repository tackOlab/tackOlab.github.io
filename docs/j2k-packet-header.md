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

# JPEG 2000 パケットヘッダの生成方法
## 符号長の表現方法
コードブロック内の符号長を表すビット列のビット長を$bits$とおく．$bits$は$Lblock$と呼ばれる値を用いて以下の式によって定義される．

\begin{align}
bits &= Lblock + \left\lfloor \log_2 \mathrm{number of oding pass to be added}\right\rfloor
\end{align}

$Lblock$の初期値は$3$であり，$Lblock$の値は増加するのみで減少はしない．コードブロック毎に独立した$Lblock$を用いる．
### 単一の符号長による表現
