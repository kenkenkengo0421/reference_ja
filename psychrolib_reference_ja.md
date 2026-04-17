# PsychroLib (version 2.5.0) 日本語リファレンス

```Bash

npm install psychrolib

```

```TypeScript
// リファレンス通りのベーシックな書き方
const psychrolib = require('psychrolib');

/* TypeScript特有の補足
もしTypeScriptの設定（tsconfig.json）によっては、上記の `require` に対して
「requireが見つかりません」というエラーが出る場合があります。
その場合は、TypeScript標準のrequire構文である以下を使用してください。
*/
// import psychrolib = require('psychrolib');
```



## 概要
PsychroLibは、気体と蒸気の混合物、および標準大気の熱力学的特性を計算するためのライブラリです。多くの工学、物理学、気象学のアプリケーションに適しています。ほとんどの関数は、2017年のASHRAE Handbook - Fundamentalsに記載されている公式を実装したもので、国際単位系（SI）とヤード・ポンド法（IP）の両方に対応しています。

### 使用例 (Node.js)
```javascript
// PsychroLibをインポート
var psychrolib = require('./psychrolib.js')

// 単位系を設定する (SI単位系)
psychrolib.SetUnitSystem(psychrolib.SI)

// 乾球温度25℃、相対湿度80%から露点温度を計算する
var TDewPoint = psychrolib.GetTDewPointFromRelHum(25.0, 0.80);

console.log('TDewPoint: %d', TDewPoint);
// 期待される出力: 21.3094
```

## グローバル定数
ライブラリ内で使用される主な定数です。

* **`ZERO_FAHRENHEIT_AS_RANKINE`**: 華氏0度（°F）をランキン度（°R）で表した値（459.67）。
* **`ZERO_CELSIUS_AS_KELVIN`**: 摂氏0度（°C）をケルビン（K）で表した値（273.15）。
* **`R_DA_IP`**: 乾燥空気の普遍気体定数（IP単位系）。
* **`R_DA_SI`**: 乾燥空気の普遍気体定数（SI単位系）。
* **`INVALID`**: 無効な値（-99999）。
* **`MAX_ITER_COUNT`**: whileループの最大反復回数（100）。
* **`MIN_HUM_RATIO`**: 許容される最小の湿度比（1e-7）。これ以下の値はすべてこの値にリセットされます。
* **`FREEZING_POINT_WATER_IP` / `SI`**: 水の氷点（IP: 32.0 / SI: 0.0）。
* **`TRIPLE_POINT_WATER_IP` / `SI`**: 水の三重点（IP: 32.018 / SI: 0.01）。

## ヘルパー関数 (単位系の設定)
ライブラリを使用する前に、必ず単位系を設定する必要があります。

* **`SetUnitSystem(UnitSystem)`**: 単位系を設定します（`this.IP` または `this.SI` を指定）。
* **`GetUnitSystem()`**: 現在使用中の単位系を返します。
* **`isIP()`**: 現在の単位系がIPかSIかを確認します。

## 計算関数一覧

### 温度単位の変換
* **`GetTRankineFromTFahrenheit(T_F)`**: 華氏（°F）からランキン度（°R）に変換します。
* **`GetTFahrenheitFromTRankine(T_R)`**: ランキン度（°R）から華氏（°F）に変換します。
* **`GetTKelvinFromTCelsius(T_C)`**: 摂氏（°C）からケルビン（K）に変換します。
* **`GetTCelsiusFromTKelvin(T_K)`**: ケルビン（K）から摂氏（°C）に変換します。

### 露点、湿球温度、相対湿度の相互変換
* **`GetTWetBulbFromTDewPoint(TDryBulb, TDewPoint, Pressure)`**: 乾球温度、露点温度、気圧から湿球温度を計算します。
* **`GetTWetBulbFromRelHum(TDryBulb, RelHum, Pressure)`**: 乾球温度、相対湿度、気圧から湿球温度を計算します。
* **`GetRelHumFromTDewPoint(TDryBulb, TDewPoint)`**: 乾球温度と露点温度から相対湿度を計算します。
* **`GetRelHumFromTWetBulb(TDryBulb, TWetBulb, Pressure)`**: 乾球温度、湿球温度、気圧から相対湿度を計算します。
* **`GetTDewPointFromRelHum(TDryBulb, RelHum)`**: 乾球温度と相対湿度から露点温度を計算します。
* **`GetTDewPointFromTWetBulb(TDryBulb, TWetBulb, Pressure)`**: 乾球温度、湿球温度、気圧から露点温度を計算します。

### 水蒸気圧の計算
* **`GetVapPresFromRelHum(TDryBulb, RelHum)`**: 相対湿度と温度から水蒸気分圧を計算します。
* **`GetRelHumFromVapPres(TDryBulb, VapPres)`**: 乾球温度と水蒸気圧から相対湿度を計算します。
* **`GetTDewPointFromVapPres(TDryBulb, VapPres)`**: 乾球温度と水蒸気圧から露点温度を計算します。
* **`GetVapPresFromTDewPoint(TDewPoint)`**: 露点温度から水蒸気圧を計算します。

### 湿度比（絶対湿度）の計算
* **`GetTWetBulbFromHumRatio(TDryBulb, HumRatio, Pressure)`**: 乾球温度、湿度比、気圧から湿球温度を計算します。
* **`GetHumRatioFromTWetBulb(TDryBulb, TWetBulb, Pressure)`**: 乾球温度、湿球温度、気圧から湿度比を計算します。
* **`GetHumRatioFromRelHum(TDryBulb, RelHum, Pressure)`**: 乾球温度、相対湿度、気圧から湿度比を計算します。
* **`GetRelHumFromHumRatio(TDryBulb, HumRatio, Pressure)`**: 乾球温度、湿度比、気圧から相対湿度を計算します。
* **`GetHumRatioFromTDewPoint(TDewPoint, Pressure)`**: 露点温度と気圧から湿度比を計算します。
* **`GetTDewPointFromHumRatio(TDryBulb, HumRatio, Pressure)`**: 乾球温度、湿度比、気圧から露点温度を計算します。
* **`GetHumRatioFromVapPres(VapPres, Pressure)`**: 水蒸気圧と大気圧から湿度比を計算します。
* **`GetVapPresFromHumRatio(HumRatio, Pressure)`**: 湿度比と気圧から水蒸気圧を計算します。
* **`GetSpecificHumFromHumRatio(HumRatio)`**: 湿度比（混合比）から比湿を計算します。
* **`GetHumRatioFromSpecificHum(SpecificHum)`**: 比湿から湿度比（混合比）を計算します。

### 乾燥空気の計算
* **`GetDryAirEnthalpy(TDryBulb)`**: 乾球温度から乾燥空気のエンタルピーを計算します。
* **`GetDryAirDensity(TDryBulb, Pressure)`**: 乾球温度と気圧から乾燥空気の密度を計算します。
* **`GetDryAirVolume(TDryBulb, Pressure)`**: 乾球温度と気圧から乾燥空気の比体積を計算します。
* **`GetTDryBulbFromEnthalpyAndHumRatio(MoistAirEnthalpy, HumRatio)`**: エンタルピーと湿度比から乾球温度を計算します。
* **`GetHumRatioFromEnthalpyAndTDryBulb(MoistAirEnthalpy, TDryBulb)`**: エンタルピーと乾球温度から湿度比を計算します。

### 飽和空気の計算
* **`GetSatVapPres(TDryBulb)`**: 乾球温度から飽和水蒸気圧を計算します。
* **`GetSatHumRatio(TDryBulb, Pressure)`**: 乾球温度と気圧から飽和空気の湿度比を計算します。
* **`GetSatAirEnthalpy(TDryBulb, Pressure)`**: 乾球温度と気圧から飽和空気のエンタルピーを計算します。

### 湿り空気の計算
* **`GetVaporPressureDeficit(TDryBulb, HumRatio, Pressure)`**: 乾球温度、湿度比、気圧から飽和差（Vapor Pressure Deficit）を計算します。
* **`GetDegreeOfSaturation(TDryBulb, HumRatio, Pressure)`**: 乾球温度、湿度比、大気圧から飽和度（unitless）を計算します。
* **`GetMoistAirEnthalpy(TDryBulb, HumRatio)`**: 乾球温度と湿度比から湿り空気のエンタルピーを計算します。
* **`GetMoistAirVolume(TDryBulb, HumRatio, Pressure)`**: 乾球温度、湿度比、気圧から湿り空気の比体積を計算します。
* **`GetTDryBulbFromMoistAirVolumeAndHumRatio(MoistAirVolume, HumRatio, Pressure)`**: 湿り空気の比体積、湿度比、気圧から乾球温度を計算します。
* **`GetMoistAirDensity(TDryBulb, HumRatio, Pressure)`**: 乾球温度、湿度比、気圧から湿り空気の密度を計算します。

### 標準大気の計算
* **`GetStandardAtmPressure(Altitude)`**: 標高（高度）から標準大気の気圧を計算します。
* **`GetStandardAtmTemperature(Altitude)`**: 標高（高度）から標準大気の温度を計算します。
* **`GetSeaLevelPressure(StnPressure, Altitude, TDryBulb)`**: 現地気圧、標高、乾球温度から海面気圧を計算します。
* **`GetStationPressure(SeaLevelPressure, Altitude, TDryBulb)`**: 海面気圧、標高、乾球温度から現地気圧（ステーション気圧）を計算します。

### 湿り空気特性の一括算出用ユーティリティ関数
これらの関数は、特定の入力値から関連するすべての湿り空気特性の配列を一括で返します。
* **`CalcPsychrometricsFromTWetBulb(TDryBulb, TWetBulb, Pressure)`**: 乾球温度、湿球温度、気圧からすべての特性を算出します。
* **`CalcPsychrometricsFromTDewPoint(TDryBulb, TDewPoint, Pressure)`**: 乾球温度、露点温度、気圧からすべての特性を算出します。
* **`CalcPsychrometricsFromRelHum(TDryBulb, RelHum, Pressure)`**: 乾球温度、相対湿度、気圧からすべての特性を算出します。

*(戻り値の配列順: `[HumRatio, 算出される温度パラメータ(TDewPoint等), RelHum, VapPres, MoistAirEnthalpy, MoistAirVolume, DegreeOfSaturation]`)*



<br>
<br>
<br>
<br>
謝辞・ライセンス
本ドキュメントおよび関連する計算処理は、PsychroLib (version 2.5.0) の公式リファレンスを基に日本語化・再構成したものです。オリジナルの実装およびASHRAEの計算式・係数は、MITライセンスの元で提供されています
