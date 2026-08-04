# Changelog

## [Unreleased]

- 公開interpolatorのparam/port doc commentを追加し、説明文の途中改行を整理
- doc commentの句点と体言止めの表記を整理
- doc commentのsummary表記を統一
- 各testのdoc commentを検証目的が分かる表現へ統一
### Added

- linear、cubic Lagrange補間kernelを独立packageへ移動
- `FarrowInterpolator`を`CubicLagrangeInterpolator`へ改名
- signed sample、Q0.phase、nearest ties to even、saturationを明示する数値APIと全rounding mode／小幅全探索testを追加
- fixedpoint project-scopeの`resize`をlinear／cubicから直接呼び出す構成へ変更

### Changed

- 破壊的変更: `LinearInterpolator`と`CubicLagrangeInterpolator`を`FORMAT` genericと`FixedPointValue::<FORMAT>` portへ移行し、既定formatをQ2.23へ変更
- `LinearInterpolator`の差分とphaseの積を、accumulatorへ事前拡張せず必要なoperand幅のまま生成するよう整理
- `ZeroOrderHold`を公開APIから外し、比較ベンチマーク専用の`BenchmarkZeroOrderHold`へ整理
