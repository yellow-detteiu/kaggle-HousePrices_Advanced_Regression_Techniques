# kaggle-HousePrices_Advanced_Regression_Techniques
初めて参加するコンペ(不動産価格予想)のkaggle日記です。

# コンペについて

### 趣旨：不動産価格の予測で、回帰モデルや特徴量作成、アンサンブルなどの技術を磨く
### データ：
SalePrice - the property's sale price in dollars. This is the target variable that you're trying to predict.  
MSSubClass: The building class  
MSZoning: The general zoning classification  
LotFrontage: Linear feet of street connected to property  
LotArea: Lot size in square feet  
Street: Type of road access  
Alley: Type of alley access  
LotShape: General shape of property  (土地の形状。通常、イレギュラーなど)
LandContour: Flatness of the property  (資産の傾斜度合い？)
Utilities: Type of utilities available  
LotConfig: Lot configuration  (土地の構成。周囲との道路の位置関係？cul-de-sac: 行きどまり)
LandSlope: Slope of property  (資産の傾斜度合い？)
Neighborhood: Physical locations within Ames city limits  
Condition1: Proximity to main road or railroad  (Proximity: 近接性)
Condition2: Proximity to main road or railroad (if a second is present)  
BldgType: Type of dwelling  (dwelling: 住宅)
HouseStyle: Style of dwelling  
OverallQual: Overall material and finish quality  
OverallCond: Overall condition rating  
YearBuilt: Original construction date  
YearRemodAdd: Remodel date  
RoofStyle: Type of roof  
RoofMatl: Roof material  
Exterior1st: Exterior covering on house  
Exterior2nd: Exterior covering on house (if more than one material)  
MasVnrType: Masonry veneer type  (石材ベニヤの種類)
MasVnrArea: Masonry veneer area in square feet  
ExterQual: Exterior material quality  
ExterCond: Present condition of the material on the exterior  
Foundation: Type of foundation  
BsmtQual: Height of the basement  
BsmtCond: General condition of the basement  
BsmtExposure: Walkout or garden level basement walls  
BsmtFinType1: Quality of basement finished area  
BsmtFinSF1: Type 1 finished square feet  
BsmtFinType2: Quality of second finished area (if present)  
BsmtFinSF2: Type 2 finished square feet  
BsmtUnfSF: Unfinished square feet of basement area  
TotalBsmtSF: Total square feet of basement area  
Heating: Type of heating  
HeatingQC: Heating quality and condition  
CentralAir: Central air conditioning  
Electrical: Electrical system  
1stFlrSF: First Floor square feet  
2ndFlrSF: Second floor square feet  
LowQualFinSF: Low quality finished square feet (all floors)  
GrLivArea: Above grade (ground) living area square feet  
BsmtFullBath: Basement full bathrooms  
BsmtHalfBath: Basement half bathrooms  
FullBath: Full bathrooms above grade  
HalfBath: Half baths above grade  
Bedroom: Number of bedrooms above basement level  
Kitchen: Number of kitchens  
KitchenQual: Kitchen quality  
TotRmsAbvGrd: Total rooms above grade (does not include bathrooms)  
Functional: Home functionality rating  
Fireplaces: Number of fireplaces  
FireplaceQu: Fireplace quality   
GarageType: Garage location  
GarageYrBlt: Year garage was built  
GarageFinish: Interior finish of the garage  
GarageCars: Size of garage in car capacity  
GarageArea: Size of garage in square feet  
GarageQual: Garage quality  
GarageCond: Garage condition  
PavedDrive: Paved driveway  
WoodDeckSF: Wood deck area in square feet  
OpenPorchSF: Open porch area in square feet  
EnclosedPorch: Enclosed porch area in square feet  
3SsnPorch: Three season porch area in square feet  
ScreenPorch: Screen porch area in square feet  
PoolArea: Pool area in square feet  
PoolQC: Pool quality  
Fence: Fence quality  
MiscFeature: Miscellaneous feature not covered in other categories  
MiscVal: $Value of miscellaneous feature  
MoSold: Month Sold  
YrSold: Year Sold  
SaleType: Type of sale  
SaleCondition: Condition of sale  

- 81カラム
- ガレージについてや接道状況など、かなり細かく特徴量が用意されている。プールについてもあるのはありがたい。  
- 追加のシートはないようだ  
- 地域は、アメリカのイリノイ州っぽい  (中央やや東部。シカゴあたり)
https://en.wikipedia.org/wiki/Bloomington_Heights,_Illinois


# 参考  
https://invest.re-ism.co.jp/column/colum208/ (不動産価格の算定方法)  
https://ja.sekaiproperty.com/article/3701/united-states-real-estate-investment-area-population-price-trend (アメリカの不動産価格についての記事)  
https://mitomi-estate.com/differences_us-japan_real-estate-appraisal/ (アメリカの価格の決定要因について)  


# 追加で作成した特徴量

HouseAge: 売れた時点での築年数。train['YrSold'] - train['YearBuilt']。SalePriceと負の相関。  
RemodHouseAge: 改修時点からの築年数。train['YrSold'] - train['YearRemodAdd']。こちらもSalePriceと負の相関  

# 結果


# コード

kaggle-I-m_Something_of_a_Painter_Myself  
├── README.md  
├── data         <---- gitで管理するデータ  
├── data_ignore  <---- .gitignoreに記述されているディレクトリ(モデルとか、特徴量とか、データセットとか)  
├── nb           <---- jupyter lab で作業したノートブック  
├── nb_download  <---- ダウンロードした公開されているkagglenb  
└── src          <---- .ipynb 以外のコード  



# 日記

## 2022/08/14
参加したのみ。モネの方をやっていたため、こちらはやっていない。


## 2022/08/15

### 概要
- コンペ開始。まずはデータ全体を眺めて特徴をつかむ 

### 気づき
- trainとtestの件数(行)は1件しか変わらない。
- trainとtestで欠損値の分布も大体同じ
- full bath, 建てられた年や改修された年, ガレージの質などがoverall conditionと相関が高い。後に建てられたものほど価格が高そう。testもほぼ同じ傾向
- SalePriceと相関が強いのは、OverallQual, GrLivArea, ガレージ系, YearBuiltなど。意外にfireplaces(暖炉)も強い
- MasVnrArea, BsmtFinSF系, 2ndFlrSF, Bath系, PoolAreaがほとんど0のため、かなり右に歪んでいる
- garage系は左に歪んでたりもする
- SalePriceも左に歪んでいる
- YearBuilt, YearRemodAddはSalePriceと明確に正の相関。築年数は負の相関。
- PoolAreaはいう程SalePriceに影響していない。プールがなくても高い家はたくさんある、が、PoolQL: Exの家は確実に高い。

### 課題、つまったこと
- trainとtestの違いをAdversarial Varidationなどで調べる必要がある
- SaleTypeの各要素がよく分からない
- そもそもこれはどの国のどの地域のデータなのか

### 学んだこと


## 2020/08/16

### 概要
データ確認の続き

### 気づき
- Street: paveが舗装。GRVLは舗装されていない道路？？paveの方が高い物件が多い
- Alley: 路地。こちらもpaveの方が高い

## 2020/08/17

### 概要
データ確認の続き

### 気づき
- RoofMatl(屋根の素材)は割と価格に差が出そう 

## 2020/08/18

### 概要
データ確認の続き。lightGBMも準備し始めた

### 気づき
- 月ごとではあまり値段の特徴なさそう  
- ベニヤ石材の面積は割と関係ありそう(なんで？？)  
https://kenzai-digest.com/entrance-approach/ 
https://harima-ie.com/tab.php?id=9950 (自然素材の家は価格が高くなる？？)  

### 課題、つまったこと  
- LandContourとLandSlopeはどう違うか。おそらく前者は周囲の道路と土地がどのくらいの傾斜でつながっているか、後者は家そのものの傾斜具合  

## 2020/08/19

### 概要
特徴量選択をするためにも、lightGBMのモデルを構築し始めるが、期限が明日に迫ってしまう。  
このコンペは諦めて、他のコンペに集中しよう。  
このコンペでの分析は少しずつ進めるが、これからは期限に猶予のあるコンペを選ぼう。  

### 課題、つまったこと  
lightGBMの予測段階でエラーが多発する。object型もあるため、処理に苦戦中。  
- 
