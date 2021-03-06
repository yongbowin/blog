**A Robust Adversarial Training Approach to Machine Reading Comprehension**
 - 一种提升阅读理解模型鲁棒性的对抗训练方法，[pdf](https://h5.clewm.net/?url=qr61.cn/o5dpyY/q4KDGPM)
 - Motivation：
    - 之前为训练集添加对抗样本是通过基于规则的方式
    - 缺点是对抗样本的类型是不计其数的，不可能一一列举
 - 基本思路：
    - 给定一个在原始语料训练好的MRC模型
    - 本文的方法基于当前的模型的参数来动态生成对抗样本
    - 通过生成的样本来进一步迭代的训练MRC模型
    - 该方式不需要事先知道任何对抗类型
 - 该模型分为一下三步：
    - （1）把MRC模型当做一个黑盒，为每一个原始sample获取一个扰动的word embedding序列，该序列可能导致MRC给出错误的预测
    - （2）从每一个扰动的序列中采样一个word序列，该序列作为误导的文本，插入到原始的sample中去创建一个对抗sample
    - （3）使用原始样本和第二步生成的扰动的样本，重新训练MRC模型，然后用重新训练好的模型重复第一步，直到模型收敛
 - 经过上述过程，希望得到一个通用的对抗模型，而不是特定类型攻击样本的对抗模型
 - 之前的增加模型鲁棒性的方法：
    - 数据增强：
        - [Jia and Liang 2017]() 设计了一些不同类型的对抗样本
        - 为了抵御那些类型的对抗样本，[Wang and Bansal 2018]() 基于规则自动创建额外的训练样本来丰富训练数据
    - 模型改进：
        - [Salant and Berant 2018]() 通过预训练语言模型的embedding作为输入，为了收集丰富的上下文语境信息
        - [Min et al. 2018]() 提出了一个句子选择器去选择最小的句子集合，为了将来的预测，以便避免许多干扰
        - [Liu et al. 2018]() 设计了一个输出层去平均多个预测，来改善模型鲁棒性
        - [Hu et al. 2018b]() 基于模型蒸馏的训练方法训练集成模型，来训练鲁棒的单模型
    - 对抗方法：
        - [Behjati et al. 2019]() 通过一个干扰的embedding试图干扰文本分类
        - [Iyyer et al. 2018)]() 提出了一个依存句法控制解释网络来生成语法对抗样本
        - [Gong et al. 2018]()和[Sato et al. 2018]() 通过训练干扰的embedding和搜索最近的tokens来生成误导文本
        - [Jia and Liang 2017]()和[Alzantot et al. 2018]() 通过在文本迭代中替换tokens直到得到一个成功的攻击来自动生成误导文本
 - 作者改进了上述方式：
    - 数据增强：寻求一种通用的方式来生成各种类型的对抗样本
    - 模型改进：没改动，基本可应用在所有可导的模型
    - 对抗方法：更通用的对抗样本生成方法
 - 对抗训练方法：
    - 和GAN类似，本文的对抗训练方法在做一个‘最小-最大’的游戏分别在对抗样本生成器和MRC模型之间
    - 训练过程如下：
        - 1.在训练干扰向量（perturbation embedding）序列时，固定阅读理解模型参数（已经训练好的，作为对抗生成器），以最小化当前模型正确预测概率为目标（正常情况下应该最大化该概率，给定问题和篇章来预测答案）
        - 2.使用贪婪算法，从perturbation embedding中采样word序列作为误导文本来创建和丰富对抗样本集合
        - 3.训练MRC模型，最大化预测概率（最大化真实答案的概率）来抵御那些对抗样本。然后用该重新训练的模型返回第一步作为新的生成器直至收敛
    - 为了全覆盖对抗样本的类型，本模型试着生成两种类型的误导文本Misleading texts：
        - 1.Misleading answer texts：misleading texts试着说服MRC模型，正确答案在该文本中
        - 2.Misleading context texts：misleading texts作为context，试着引导MRC模型从正确答案转移注意到错误的答案
    - 本文的方法也试图控制误导性文本是相似的或不同的问题，而大多数已知的误导性文本主要是与问题相关的
 - MRC模型定义忽略，目标是求开始位置和结束位置的最大概率分别所在的位置
 - 扰动向量训练（Perturbation Embedding Training）
    - 目的是在目标模型的监督下，为每一个实例训练一个扰动embedding序列
    - 在训练过程中，把模型当作一个生成器，所有的模型参数都是固定的
    - 训练模型用一个额外的扰动embedding序列去扰动每一个输入passage
    - 对每一个训练实例，首先插入一个连续的扰动embedding序列`e'`到passage的embedding序列`e_p`，采用串联操作，因此passage的embedding序列变成：
        
        ![pert_form](img/pert_form.png)
    - 为了后续采样的方便性，需要限制`e'`的搜索空间，`e'`是词汇表embedding的权重和
        
        ![pert_form_1](img/pert_form_1.png)
    - 其中`w属于R^{l×|V|}`作为`e'`的权重矩阵，`e'`的长度为l，`w_i属于R^{|V|}`是`e'`的权重向量
    
        ![pert_form_2](img/pert_form_2.png)
    - 为了让权重向量`w_i`在词汇表空间中标准化，定义`w_{ij}`作为可训练参数`a属于R^{l×|V|}`的softmax结果
    - 也就是说，对于每个实例，训练一个扰动embedding序列就是找到一个合适的权重矩阵`w`的分布
    - 生成misleading answer texts：
        - 为了干扰MRC模型，设计一个交叉熵损失函数，目的是欺骗MRC模型使得模型相信答案位于扰动向量`e'`中：
            
            ![pert_form_3](img/pert_form_3.png)
            
            其中![pert_form_3_1](img/pert_form_3_1.png)是一个位于扰动向量序列中的一个扰动答案
    - 生成misleading context texts：
        - 设计一个损失函数，最小化模型在ground truth span ![pert_form_3_2](img/pert_form_3_2.png)上的估计，目的是为了扰动MRC模型
        
            ![pert_form_4](img/pert_form_4.png)
    - 训练损失函数如下：
        
        ![pert_form_5](img/pert_form_5.png)
        
    - 其中`R_s`是正则化项，是一个相似度的正则化，为了控制然动向量（ perturbation embeddings）和question & answers之间的相似度
    
        ![pert_form_6](img/pert_form_6.png)
    - `e_a`表示答案句子的embedding序列，`sim(·,·)`表示BOW余弦函数：
        
        ![pert_form_7](img/pert_form_7.png)
    - 训练模型训练一个被w定义的扰动向量`e'`，最小化损失L，E‘就是目标扰动向量序列
        
        ![pert_form_8](img/pert_form_8.png)
 - 模型框架如下：
 
    ![pert_arch](img/pert_arch.png)
 
    - 给定的扰动span `s_d`和ground truth `s_g`是监督信号
    - 重复训练直到收敛或者L低于阈值，返回权重矩阵w供后续采样
 - 贪婪采样
    - 对`E'`的每一个位置进行贪婪采样，使用欧式距离进行计算
    - 即计算`e'_i = sum (w_{ij} v_j)`与词汇表中哪个词距离最近
    - 使用贪婪采样，简化地取具有最大权重的token
        
        ![pert_greedy](img/pert_greedy.png)
    - 因此，对于每一个实例，生成一个misleading text就是采样一个具有最大权重的token序列`A=t'_1t'_2...t'_l`，从训练好的w
 - 使用对抗样本丰富后的数据集来重新训练模型
 - 训练策略：
    
    ![pert_train_strategy](img/pert_train_strategy.png)
    
    - 每一次迭代，对抗训练都从全部训练数据集中随机采样一个子数据集进行训练
    - 对于perturbation embedding training process，使用给定的模型参数（训练好的）来训练权重矩阵w，并收集它的平均训练损失
    - 进行贪婪采样得到A，并将其插入到passage中得到（或创建）对抗样本
    - 使用上一步得到的丰富后的训练集重新训练模型
    - 训练终止的条件是损失小于阈值或者到达T


**Exploring the Limits of Language Modeling**
 - 论文时间：2016年2月7日， [pdf](https://arxiv.org/pdf/1602.02410v1.pdf)
 - 扩展已有的模型去处理两个关键的任务：
    - 语料库和词汇尺寸
    - 以及复杂的、长期依赖的语言结构
 - seq2seq使用的是条件语言模型
 - 强条件独立型假设是不切实际的，像n-gram仅仅利用前边词的短的历史信息来预测下一个词
 - 本文的主要贡献：
    - 大规模的LM
    - 基于character level CNNs设计一个softmax loss，精度和full softmax一样，但是具有更多的参数量
    - 比之前的模型减少了20倍参数量，困惑度从51.3下降到30.0，集成模型困惑度下降到23.7
 - **LM的目标是学习与语言相关的符号序列的概率分布**。
 - 已有的工作：
    - 参数化的方法：
        - log-linear models
    - 非参数化的方法：
        - count-based LMs：基于N-grams的统计，增加平滑项（考虑未见过的序列），缺点是不能考虑长距离的依赖
 - 模型架构：
 
    ![ellm](img/ellm.png)
    
    - （a）是一个标准的LSTM LM
    - （b）表示一个输入和softmax embedding被一个character CNN取代的语言模型
    - （c）将softmax用一个下一个character预测的LSTM网络替换
 - 采用10亿个词进行训练，相对count-based模型是中等大小，对于NN-based来说就是巨大的量级了
 - 卷积嵌入模型（Convolutional Embedding Models）
    - 参考：[Character-Aware Neural Language Models](https://arxiv.org/pdf/1508.06615.pdf)
    - character经过一个1-d CNN处理，对每一个卷积特征用max-pooling横跨整个序列
    - 将结果特征输入到一个2-layer highway network
 - 在大词汇表上应用softmax（Softmax Over Large Vocabularies）
    - 一般地，预测通过softmax non-linearity：
        
        ![ellm_form](img/ellm_form.png)
                
        - `z_w`是logit，对应一个词w
        - logit一般使用公式：
            
            ![ellm_form_1](img/ellm_form_1.png)
            
            进行计算，其中h是一个context vector，`e_w`是w的word embedding
    - 如果词汇边很大，计算量会相当巨大，使用以下方式减小计算量：
        - importance sampling：重要性采样（本文采用此方法）
        - Noise Contrastive Estimation（NCE）：噪声对比估计
        - self normalizing partition functions：自规范化分区函数
        - Hierarchical Softmax：分层softmax
 - 语言模型改进：
    - 基于RNN的语言模型采用链式规则在词序列上去建模联合概率
        
        ![ellm_form_2](img/ellm_form_2.png)
        
        所有之前的词的上下文信息使用LSTM去编码，用softmax计算在词上的概率
    - 噪声对比估计和重要性采样之间的关系：
        - 分层softmax采用一个树结构，词上的概率分布被分解成两个概率的乘积（对于每个词），大大节省了时间，因为只有制定路径的层次需要被计算和更新，选择一个好的层次很重要（本文不采用这种方法）
        - 采样方法仅仅用在训练的时候，因为它们的目的是对loss做一个近似，方便计算，但是在推断的时候还是要计算所有词的规范化项
        - 噪声对比估计考虑用一个代理的二分类任务，该分类器被训练去区分真实数据和从任意分布中采样的数据
    - CNN softmax：
        - xx









**Deep contextualized word representations**
 - 论文时间：2018年3月22日， [pdf](https://arxiv.org/pdf/1802.05365.pdf)
 - 介绍：
    - 即ELMo (Embeddings from Language Models)，词向量是一个深度双向语言模型（biLM）的内部状态的函数，在大规模的语料上进行预训练
    - ELMo的表示是深层次的，是所有biLM内部层的函数
    - 高层的LSTM状态捕获词义的上下文层面含义，它们不经过修改就可以被用到词义消歧任务，低层的状态对语法的层面建模，它们可以被用到词性标注
    - 实验表明深层表示胜过那些仅仅从LSTM顶层派生出来的结果
    - 大规模预训练的词向量可以在无标注的文本上进行训练，可以捕获到句法和语义的信息，但是那些方法对每个词仅仅允许一个单个的上下文无关文法的表示
    - 之前的方法通过子词信息或者学习词义的分离的向量来克服缺点，本文的方法也获益于character convolutions
    - 最近其它方法在关注上下文依赖的表示
        - context2vec：用一个BiLSTM去编码中心词周围的上下文
    - 使用3000万的句子来训练biLM
    - biRNN的不同层编码信息的不同类型，例如引入多人物的句法监督（如词性标注）在深度LSTM的低层能够改善高层任务的总体的性能（如依赖关系分析）
    - 在一个两层的LSTM中，第一层的表示在做词性标注任务时表现的比第二层好
 - 双向语言模型：
    - 前向语言模型通过给定的历史词信息来建模词的概率，进而来计算句子序列的概率
    - 在每一个位置k，每一个LSTM层输出一个上下文依赖的表示 ![elmo_form_3](img/elmo_form_3.png)，顶层的LSTM输出经过一个softmax层被用来预测下一个词`t_{k+1}`
        - 正向
        
            ![elmo_form](img/elmo_form.png)
        - 反向
        
            ![elmo_form_1](img/elmo_form_1.png)
        - biLM结合了正向和反向语言模型，联合正向和反向最大化log似然求解：
        
            ![elmo_form_2](img/elmo_form_2.png)
            
            `theta_x`是token表示的参数，`theta_s`是softmax层的参数
 - ELMo：
    - ELMo是一个用biLM中间层表示的特定任务的组合
    - 对于每一个token `t_k`，一个L层的biLM计算一个包含`2L+1`个表示的集合：
        
        ![elmo_form_4](img/elmo_form_4.png)
        
        当`j=0`时是token层
    - 对于下游的模型，ELMo折叠在R中的所有的层到一个单个的向量表示：
    
        ![elmo_form_5](img/elmo_form_5.png)
        
        `s^{task}`是softmax-normalized weights，缩放参数`gama^{task}`允许任务模型缩放整个ELMo向量，`gama`对于优化过程很重要
    - 考虑到每一个biLM层的激活函数有不同的分布，在一些情况下，在weighting之前对每一个biLM层使用LayerNorm是有帮助的
 - 将biLM用在有监督NLP任务上
    - 记录每个词在每一层的表示，然后让端任务对那些表示去学习一个线性的结合
    - 大部分NLP监督模型共享最底层的架构，允许我们用一致的和统一的方式去增加ELMo
    - 首先冻结biLM的权重，然后将`x_k`和ELMo向量进行连接：
    
        ![elmo_form_6](img/elmo_form_6.png)
    - 增加一定数量的dropout和正则化ELMo权重（例如在loss中增加`lambda ||w||_{2}^2`）
 - pre-trained biLMs
    - 相比已有的模型，增加支持双向联合训练和在LSTM层之间增加一个一个残差连接
    - 减半所有的embedding和hidden维度从单个最好的模型[CNN-BIG-LSTM](https://arxiv.org/pdf/1602.02410.pdf)，最终的模型使用2层的BiLSTM，4096个单元、512维度、从第一层到第二层的一个残差连接
    - 上下文不敏感的类型表示（也就是编码层）使用2048个character n-gram卷积过滤器，再加两层highway network，经过一个线性映射将维度转化为512
    - 也就是说biLM为每一个token提供三层的表示（对于域外词也会有很好的表示）
 - 分析
    - 低层捕获句法信息，高层捕获语义信息
    - biLM的不同层表示不同类型的信息


**为啥固定biLM的权重？**








































