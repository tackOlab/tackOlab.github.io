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

1. 最初の符号バイトに対応する符号化パス数は1なので，$bits = Lblock + \left\lfloor\log_2(1)\right\rfloor = 3$．
6は3ビットで表現できるので，$Lblock$の増加はなし(0)．よって3ビットで6バイト(110)を表現する．$Lblock=3$．

1. 2番目の符号バイトに対応する符号化パス数は9なので，
$bits = Lblock + \left\lfloor\log_2(9)\right\rfloor = 6$．
31は6ビットで表現できるので，$Lblock$の増加はなし(0)．よって6ビットで31バイト(01 1111)を表現する．$Lblock=3$．

1. 3番目の符号バイトに対応する符号化パス数は2なので，
$bits = Lblock + \left\lfloor\log_2(2)\right\rfloor = 4$．
44は4ビットで表現できないので（6ビット必要），$Lblock$を$6-4=2$増加させる(110)．6ビットで44バイト(10 1100)を表現する．$Lblock=3+2=5$．

1. 4番目の符号バイトに対応する符号化パス数は5なので，
$bits = Lblock(=5) + \left\lfloor\log_2(5)\right\rfloor = 7$．
134は7ビットで表現できないので（8ビット必要），$Lblock$を$8-7=1$増加させる(10)．8ビットで134バイト(1000 0110)を表現する．$Lblock=5+1=6$．

### 複数の符号長による表現

複数の符号長が単一のパケットに含まれる場合の例を以下に示す．

- JPEG 2000 Part 1で定義されるBYPASS MODE（Selective arithmetic coding bypass）では，ゼロビットプレーンでないビットプレーンの最上位を1とし，4番目までのビットプレーンではClean Up(CU)，Significance Propagation(SP)，Magnitude Refinment(MR)の各符号化パスは算術符号化され，5番目以降のビットプレーンにおいてはSP, MRは生(RAW)のビット情報が記録される．5番目以降のビットプレーンであっても，CUパスは算術符号化される．4番目ビットプレーンの最後，CUパスで算術符号化の終端処理(termination)が行われる．また，5番目以降のビットプレーンでは，RAWと算術符号化の境目（すなわち，RAWのMRパスの後と，CUパスの後）にterminationされる．各終端処理で区切られた符号バイトをセグメントと呼ぶ．複数のセグメントが単一のパケットに存在する場合，複数の符号長による表現が必要となる．
- HTJ2Kにおいて，SPあるいはMR，またはその両方(HT Magref segment)が存在する場合，HT Cleanup segmentと合わせて複数のセグメントが単一のパケットに存在することになり，複数の符号長による表現が必要となる．
- 未検証（HTJ2Kにおいて，Placeholder passが存在する場合．）

#### 例

今，コードブロック内に，4つのセグメントが存在するとしよう，各セグメントの符号バイト数は{6, 75, 134, 192}，各セグメントを構成する符号化パス数は{1, 2, 1, 1}とする．これを表現するパケットヘッダの生成手順を以下に示す．

1. 複数の符号長を表現する場合，はじめに，最も大きな符号長をもつセグメントの長さを用いて$Lblock$を増加させる必要がある．すなわち，$\left\lceil\log_2(192)\right\rceil=8$なので，$Lblock$を$3$から$8$に増加させる(111110)．最初のセグメントのパス数は1なので，$bits = Lblock + \left\lfloor\log_2(1)\right\rfloor = 8$で，8ビットを使って6を表現する．(0000 0110)．
   
2. 2番目のセグメントのパス数は2なので，$bits = Lblock + \left\lfloor\log_2(2)\right\rfloor = 9$．75バイトは9ビットで表現可能なので，$Lblock$の増加はなし(0)．75を9ビットを使って表現する(0 0100 1011)．
   
3. 3番目のセグメントのパス数は1なので，$bits = Lblock + \left\lfloor\log_2(1)\right\rfloor = 8$．134バイトは8ビットで表現可能なので，$Lblock$の増加はなし(0)．134を8ビットを使って表現する(1000 0110)．
   
4. 4番目のセグメントのパス数は1なので，$bits = Lblock + \left\lfloor\log_2(1)\right\rfloor = 8$．192バイトは8ビットで表現可能なので，$Lblock$の増加はなし(0)．192を8ビットを使って表現する(1100 0000)．
