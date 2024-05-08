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
bits &= Lblock + \left\lfloor \log_2(\mathrm{num_passes})\right\rfloor
\end{align}

num_passesは，追加される符号化パス数．

$Lblock$の初期値は$3$であり，$Lblock$の値は増加するのみで減少はしない．コードブロック毎に独立した$Lblock$を用いる．

### 単一の符号長による表現

- 各コードブロックがあるパケットに寄与するバイト数は，必要に応じて$Lblock$の値を増加させるシグナリングビットが先行する．
- $0$のシグナリングビットは，$Lblock$の現在の値が十分であることを示す．
- $k$個の$1$の後に$0$が続く場合，$Lblock$の値は$k$だけ増加する．

#### 例

今，連続するレイヤ（パケットの構成単位となる）に含まれる符号のバイト数を6，31，44，134とする．各符号バイトに対応する符号化パス数を1，9，2，5とする．これを表現するパケットヘッダの生成手順を以下に示す．

##### 最初の符号バイトに対応する符号化パス数は1なので，
   
```math
bits = Lblock + \left\lfloor\log_2(1)\right\rfloor = 3
```

6は3ビットで表現できるので，$Lblock$の増加はなし(0)．よって3ビットで6バイト(110)を表現する．$Lblock=3$．

##### 2番目の符号バイトに対応する符号化パス数は9なので，

\begin{align}
bits &= Lblock + \left\lfloor\log_2(9)\right\rfloor = 6
\end{align}

31は6ビットで表現できるので，$Lblock$の増加はなし(0)．よって6ビットで31バイト(011111)を表現する．$Lblock=3$．

##### 3番目の符号バイトに対応する符号化パス数は2なので，

\begin{align}
bits &= Lblock + \left\lfloor\log_2(2)\right\rfloor = 4
\end{align}

44は4ビットで表現できないので（6ビット必要），$Lblock$を$6-4=2$増加させる(110)．6ビットで44バイト(101100)を表現する．$Lblock=3+2=5$．

##### 4番目の符号バイトに対応する符号化パス数は5なので，

\begin{align}
bits &= Lblock(=5) + \left\lfloor\log_2(5)\right\rfloor = 7
\end{align}

134は7ビットで表現できないので（8ビット必要），$Lblock$を$8-7=1$増加させる(10)．8ビットで134バイト(10000110)を表現する．$Lblock=5+1=6$．
