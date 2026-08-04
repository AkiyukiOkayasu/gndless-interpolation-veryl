# gndless_interpolation

sample windowを受け取るcombinational補間kernelです。ASRCのFIFO、phase accumulator、filterは所有しません。

公開APIは`LinearInterpolator`と`CubicLagrangeInterpolator`です。sample portは
`FixedPointValue::<FORMAT>`でformatを指定し、既定はASRC処理用のQ2.23です。phaseはunsigned
Q0.`PHASE_WIDTH`で、全方式のlatencyは0です。0次ホールドは公開kernelにせず、比較ベンチマーク内の`BenchmarkZeroOrderHold`としてのみ保持します。

linearはphase 0〜最大をsample0からsample1へ線形移動し、default roundingはnearest ties to evenです。cubicは`sample_m1`、`sample0`、`sample1`、`sample2`の4点3次LagrangeをHorner形式で評価し、全幅演算後に一度だけ丸め、default overflowはsaturationです。係数はQ2.16量子化です。

linearとcubicの最終的な幅変換には、module parameterを直接受け取れる
`gndless_fixedpoint::resize::<...>`を使用します。

```veryl
inst interp: interpolation::CubicLagrangeInterpolator (...);
```

検証: `veryl fmt --check && veryl check && veryl test && veryl build && veryl doc`。benchmarkは固定vectorのみを使い、oscillatorへ依存しません。`interpolator_benchmark`はignored Native testとして48点量子化正弦波（0.125fs、0.25fs、20/48fs相当）をlinear/cubicへ同一window・256位相で入力し、`target/interpolator_benchmark.csv`を生成します。`tools/analyze_interpolator_benchmark.py`で理想連続正弦波との誤差、方式間差、インパルス周波数応答を比較できます。
