# Changelog

## [0.3.0] - 2026-08-07

### Added

- linear、cubic Lagrange補間kernelを独立packageへ移動
- `FarrowInterpolator`を`CubicLagrangeInterpolator`へ改名
- signed sample、Q0.phase、nearest ties to even、saturationを明示する数値APIと全rounding mode／小幅全探索testを追加
- fixedpoint project-scopeの`resize`をlinear／cubicから直接呼び出す構成へ変更

### Changed

- `gndless_fixedpoint`依存を公開済みの0.2.2へ更新
- `ZeroOrderHold`を公開APIから外し、比較ベンチマーク専用の`BenchmarkZeroOrderHold`へ整理

## BREAKING CHANGE

- `LinearInterpolator`と`CubicLagrangeInterpolator`を`FORMAT` genericと`FixedPointValue::<FORMAT>` portへ移行し、既定formatをQ4.23へ変更
- `LinearInterpolatorCore`の`delta`をSAMPLE_WIDTH幅のwrap減算へ変更し、|sample1 - sample0| < 2^(SAMPLE_WIDTH - 1)の入力契約をdoc commentへ明記した。既定のQ4.23(27bit)ではdelta×phase_extが27x36乗算器1スライスへ収まる
