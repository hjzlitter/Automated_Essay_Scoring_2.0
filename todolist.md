* 看看vector方法，有什么能多造一些特征的方法。
* 伪标签？
* 扩展数据集？增加 5 6score的。或者看看之前AI_detect比赛的方法。
* lgbm？是否可以textCNN?
* fold的影响？
* 实际提交从降到0.812,他们的方法。
* 改tfidf -> char_wb 从0.812->0.811
* 改split 从15->5， 从0.812->0.810
* 0.8066
* 两个都改char_wb会到0.814 0.812->0.814
* 现在看来countvectorlizer->char_wb比较好，原因可能是因为带有比较之类的用的多的通常score高些

