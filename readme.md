# 比赛介绍
给出一段学生写的作文，对其进行打分。
给的train内容是编号 + 文本 + score（就是给作文打分）
* 编号与score似乎是没有关系的，感觉这个比赛可能不太会有数据泄漏的问题。


优化目标：
1是最优奖，用koppa作为评价标准。
2是效率奖，主办方自己造的一个评分规则：koppa与时间线性组合。

排行榜只是33%数据！！！！因此波动性会非常大，一定要注意泛化性！！
# 发展顺序
最早是lgbm，仅仅使用了paragraph、 sentence、word 词频等feature。
然后加上了tfdif，再然后加上了countvectorizer, 这时候差不多0.801了。

| Method | LB |
| --- | :---: |
| LGBM 5 fold | 0.801 |
| LGBM 15 fold | 0.802 |
| LGBM 15 fold + post-processing | 0.803 |
| LGBM 15 fold + 5 fold Deberta (Ensemble) | 0.807 |
| LGBM 16 fold + 5 fold Deberta (Ensemble) | 0.808 |
| LGBM 15 fold + 5 fold Deberta (Ensemble) + CountVectorizer | 0.810 |
| LGBM 15 fold + 5 fold Deberta (as features) + CountVectorizer | 0.811 |
| LGBM 16 fold + 5 fold Deberta (as features) + CountVectorizer + HashingVectorizer | 0.811 |
| LGBM 15 fold + 5 fold Deberta (as features) + TfidfVectorizer(ngram(3,6)) + CountVectorizer | 0.812 |
| LGBM 15 fold + (new)5 fold Deberta (as features) + TfidfVectorizer(ngram(3,6)) + CountVectorizer(ngram(3,5)) | 0.816 |


* 2024/04/15 : forked original great work kernels
    * https://www.kaggle.com/code/olyatsimboy/5-fold-deberta-lgbm
    * https://www.kaggle.com/code/aikhmelnytskyy/quick-start-lgbm
    * https://www.kaggle.com/code/hideyukizushi/aes2-5folddeberta-lgbm-countvectorizer-lb-810
    * https://www.kaggle.com/code/olyatsimboy/81-1-aes2-5folddeberta-lgbm-stacking
    

* 2024/04/16 : ~~add HashingVectorizer~~ (not work)
* 2024/04/21 : Add MetaFEs. Train deberta-v3-large local(5Fold SKF) https://www.kaggle.com/datasets/hideyukizushi/aes2-400-20240419134941
* 2024/04/22 : **My Contriduction:** change TfidfVectorizer ngram to (3,6), CountVectorizer ngram to (3,5)

---
# 可关注的人
* Chris Deotte
此人在AIMO里面也提供了很多思路。这人非常擅长此类小数据比赛，几十个金牌来着。
*  
# 一些代研究问题，可能有用的方向
* 这些作文的topic有哪些吗？
* 是否作文某些情感会造成score偏移？
* 如果按中文逻辑来说，高级词多 分数高的可能性更高一点，我们是不是要用一些模型来分辨？（就是说可能能更有针对性地微调llm）

# 本次比赛总体思路
可以看看AI text detect比赛找灵感。
* 数据增强
  - 数据集扩充。本次问题里面score有明显集中， 3 4 分的远多于1 2 5 6分的，导致其预测很差。2024/04/23 11:00 Chris Deotte给出一个扩充：https://www.kaggle.com/competitions/learning-agency-lab-automated-essay-scoring-2/discussion/496906。https://www.kaggle.com/competitions/learning-agency-lab-automated-essay-scoring-2/discussion/493962。
  - 也许可出同一内容的作文，互相拼接？
  - 替换词语？
  - 总之可以再看之前比赛学习
* 特征提取 
  - 尝试下各种vectorizer技术。
  - 自己研究文本与score的关系。本次比赛排行榜上的都非常同质化，因此可能提高预测能力的是一个比较特别的特征需要细细挖掘。
  - 使用大模型，可微调提取（https://www.kaggle.com/competitions/learning-agency-lab-automated-essay-scoring-2/discussion/494935）
- 模型
  - lgbm与textCNN相比谁更快？感觉textCNN可能在提取前后文同特征上这点更具有优势？而且CNN速度也快。
  - gru /lstm看也有人用，不知道效果如何。
- inference 技术
  - 似乎可以用int
  - loss什么最好？
  - 参数什么最好？（目前只是大概一说）
 