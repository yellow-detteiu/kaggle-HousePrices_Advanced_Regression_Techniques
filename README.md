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
LotShape: General shape of property  
LandContour: Flatness of the property  
Utilities: Type of utilities available  
LotConfig: Lot configuration  
LandSlope: Slope of property  
Neighborhood: Physical locations within Ames city limits  
Condition1: Proximity to main road or railroad  
Condition2: Proximity to main road or railroad (if a second is present)  
BldgType: Type of dwelling  
HouseStyle: Style of dwelling  
OverallQual: Overall material and finish quality  
OverallCond: Overall condition rating  
YearBuilt: Original construction date  
YearRemodAdd: Remodel date  
RoofStyle: Type of roof  
RoofMatl: Roof material  
Exterior1st: Exterior covering on house  
Exterior2nd: Exterior covering on house (if more than one material)  
MasVnrType: Masonry veneer type  
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
‐ trainとtestの件数(行)は1件しか変わらない。
- trainとtestで欠損値の分布も大体同じ
- full bath, 建てられた年や改修された年, ガレージの質などがoverall conditionと相関が高い。後に建てられたものほど価格が高そう。testもほぼ同じ傾向
- SalePriceと相関が強いのは、OverallQual, GrLivArea, ガレージ系, YearBuiltなど。意外にfireplaces(暖炉)も強い
- MasVnrArea, BsmtFinSF系, 2ndFlrSF, Bath系, PoolAreaがほとんど0のため、かなり右に歪んでいる
- garage系は左に歪んでたりもする
- SalePriceも左に歪んでいる
- YearBuilt, YearRemodAddはSalePriceと明確に正の相関。築年数は負の相関。
- PoolAreaはいう程SalePriceに影響していない。プールがなくても高い家はたくさんある、が、PoolQL: Exの家は確実に高い。
- 


### 課題、つまったこと
- trainとtestの違いをAdversarial Varidationなどで調べる必要がある
- SaleTypeの各要素がよく分からない
- そもそもこれはどの国のどの地域のデータなのか

### 学んだこと
