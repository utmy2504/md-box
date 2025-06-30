# プロトコル：HEK293細胞におけるリボスイッチアッセイ
### ref
1. Fukunaga, K., Dhamodharan, V., Miyahira, N., Nomura, Y., Mustafina, K., Oosumi, Y., Takayama, K., Kanai, A., Yokobayashi, Y., 2023. Small-Molecule Aptamer for Regulating RNA Functions in Mammalian Cells and Animals. J. Am. Chem. Soc. 145, 7820–7828. https://doi.org/10.1021/jacs.2c12332


## 1. 細胞培養と播種
1. **HEK293細胞**を、10% FBS、2 mM L-グルタミン、100 units/ml ペニシリン・ストレプトマイシンを含む**DMEM培地**で培養する。
2. 細胞は37°C、5% CO₂条件下で維持し、90%コンフルエントに達したら継代する。
3. トランスフェクションの約20時間前に、細胞をトリプシン処理で剥がし、**2.7 × 10⁵ cells/ml** の濃度に希釈する。
4. 96ウェルプレートに、1ウェルあたり**100 μl**ずつ細胞懸濁液を播種する。

## 2. トランスフェクション
1. 以下のプラスミドと試薬を混合し、メーカーの指示に従って各ウェルにトランスフェクションを行う。
    - **EGFP-aptazyme plasmid**: 100 ng
    - **pCMV-mCherry5 plasmid**: 20 ng (トランスフェクションコントロール)
    - **TransIT-293 Transfection Reagent**: 0.3 μl

## 3. リガンド添加
1. トランスフェクションから**5時間後**、各ウェルの培地を、リガンド（ASP2905またはASP7967、終濃度最大5 μM）を含む、または含まない新しい培地に交換する。
    - **注**: リガンドはDMSOに1000倍濃度で溶解しておく。

## 4. 蛍光測定
1. トランスフェクションから**48時間後**、各ウェルの培地を**100 μlのPBS**に交換する。
2. マイクロプレートリーダー（Infinite M1000 PRO）を用いて、以下の設定で蛍光強度を測定する。
    - **EGFP**: 励起 484 nm / 蛍光 510 nm (バンド幅 5 nm)
    - **mCherry**: 励起 587 nm / 蛍光 610 nm (バンド幅 10 nm)
