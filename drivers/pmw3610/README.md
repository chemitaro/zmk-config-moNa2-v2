# PMW3610 driver (vendored, patched)

[badjeff/zmk-pmw3610-driver](https://github.com/badjeff/zmk-pmw3610-driver) の `zmk-0.3` ブランチ（`5c5af40de4d8cdf55dc63c4c5907af1e52da6a95`）を取り込み、
moNa2 の無線（BLE）用に1点だけ修正したものです。元のソースは MIT ライセンス（The ZMK Contributors）です。

## 修正点

元のドライバは `CONFIG_PMW3610_REPORT_INTERVAL_MIN` を設定すると、次のサンプルが間隔以上あとに来たとき、
送っていない移動量を捨てていました。ゆっくり動かしたときや、止める直前の動きが失われます。

この版では捨てずに貯め、間隔が来たら `flush_work` でまとめて1件として送ります。
1間隔あたり最大1件しか送らないので、BLE の通信が悪くなっても送信待ちの行列が伸びにくく、カーソルの遅れがたまりません。

元のドライバとの差分は `src/pixart.h` と `src/pmw3610.c` だけです。
