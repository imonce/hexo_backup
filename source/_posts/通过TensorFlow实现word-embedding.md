---
title: 通过TensorFlow实现word embedding
date: 2019-10-21 10:16:01
tags: [TensorFlow, word2vec, n-gram, skip-gram, embedding]
---

word2vec的方法主要分为CBOW（Continuous Bag Of Words）和skip-gram（n-gram）两大类。

两种方法互为镜像。简单来说，CBOW是通过上下文预测中间值来进行训练的，skip-gram是通过中间值预测上下文来进行训练的。

这里，我们使用skip-gram的方法。

# jupyter notebook 脚本


```
from __future__ import absolute_import
from __future__ import division
from __future__ import print_function

from sklearn.manifold import TSNE
import matplotlib.pyplot as plt

import collections
import math
import os
import random
import zipfile

import numpy as np
from six.moves import urllib
from six.moves import xrange
import tensorflow as tf
```

    /Users/i/anaconda/lib/python3.6/site-packages/h5py/__init__.py:34: FutureWarning: Conversion of the second argument of issubdtype from `float` to `np.floating` is deprecated. In future, it will be treated as `np.float64 == np.dtype(float).type`.
      from ._conv import register_converters as _register_converters



```
class BasicPatternEmbedding:
    def __init__(self):
        self.url = 'http://mattmahoney.net/dc/'
        self.data_index = 0
        
        self.vocabulary_size = 5000
        
        self.batch_size = 128
        self.embedding_size = 128  # Dimension of the embedding vector.
        self.skip_window = 1       # How many words to consider left and right.
        self.num_skips = 2         # How many times to reuse an input to generate a label.

        # We pick a random validation set to sample nearest neighbors. Here we limit the
        # validation samples to the words that have a low numeric ID, which by
        # construction are also the most frequent.
        self.valid_size = 16     # Random set of words to evaluate similarity on.
        self.valid_window = 100  # Only pick dev samples in the head of the distribution.
        # choose 16 numbers from 0 to 99 randomly
        self.valid_examples = np.random.choice(self.valid_window, self.valid_size, replace=False)
        self.num_sampled = 64    # Number of negative examples to sample.
        self.num_steps = 10001
        
        self.final_embedding = None
        
        self.graph = tf.Graph()
        
    # download and verify the dataset file
    def maybe_download(self, filename, expected_bytes):
        # If the dataset file is not under the current path, download it directly
        if not os.path.exists(filename):
            filename, _ = urllib.request.urlretrieve(self.url + filename, filename)
        # get dataset file infomationn
        statinfo = os.stat(filename)
        # verify file size
        if statinfo.st_size == expected_bytes:
            print('Found and verified', filename)
        else:
            print(statinfo.st_size)
            raise Exception(
                'Failed to verify ' + filename + '. Can you get to it with a browser?')
        return filename
    
    # read the data from zip into a list of strings
    def read_data(self, filename):
        with zipfile.ZipFile(filename) as f:
            # separate by default separators, that is, all null characters, including spaces, newlines (\n), tabs (\t), etc.
            data = tf.compat.as_str(f.read(f.namelist()[0])).split()
        return data
    
    # process raw inputs into a dataset
    def build_dataset(self, words):
        # add unknown words into count list
        count = [['UNK', -1]]
        # count the words list and add the pairs (word_name, number) into count list
        count.extend(collections.Counter(words).most_common(self.vocabulary_size - 1))
        dictionary = dict()
        # create a dictionary of the words with serial number
        for word, _ in count:
            dictionary[word] = len(dictionary)
        data = list()
        unk_count = 0
        # convert the word list into a number list, 0 for unknown words
        for word in words:
            if word in dictionary:
                index = dictionary[word]
            else:
                index = 0
                unk_count += 1
            data.append(index)
        # update the number of UNK
        count[0][1] = unk_count
        # generate a new dictionary by exchanging key and value
        reversed_dictionary = dict(zip(dictionary.values(), dictionary.keys()))
        return data, count, dictionary, reversed_dictionary
    
    # function to generate a training batch for the skip-gram model
    def generate_batch(self, data):
        # make sure the data length is OK
        assert self.batch_size % self.num_skips == 0
        assert self.num_skips <= 2 * self.skip_window

        batch = np.ndarray(shape=(self.batch_size), dtype=np.int32)
        labels = np.ndarray(shape=(self.batch_size, 1), dtype=np.int32)
        span = 2 * self.skip_window + 1  # [ skip_window target skip_window ]
        # create a new double-ended queue to store the buffer
        buffer = collections.deque(maxlen=span)
        # data_index indicates the end point of the current window
        if self.data_index + span > len(data):
            data_index = 0
        buffer.extend(data[self.data_index:self.data_index + span])
        self.data_index += span
        for i in range(self.batch_size // self.num_skips):
            target = self.skip_window  # target label at the center of the buffer
            targets_to_avoid = [self.skip_window]
            # sample num_skips batches and labels, optimizable
            for j in range(self.num_skips):
                while target in targets_to_avoid:
                    target = random.randint(0, span - 1)
                # avoid sampling to the same target
                targets_to_avoid.append(target)
                # each batch item stands for input
                batch[i * self.num_skips + j] = buffer[self.skip_window]
                # each label item stands for ground truth
                labels[i * self.num_skips + j, 0] = buffer[target]
            if self.data_index == len(data):
                buffer[:] = data[:span]
                self.data_index = span
            else:
                buffer.append(data[self.data_index])
                self.data_index += 1
        # Backtrack a little bit to avoid skipping words in the end of a batch
        self.data_index = self.data_index - span
        return batch, labels
    
    def train(self, data, reverse_dictionary):
        with self.graph.as_default():
            train_inputs = tf.placeholder(tf.int32, shape=[self.batch_size])
            train_labels = tf.placeholder(tf.int32, shape=[self.batch_size, 1])
            valid_dataset = tf.constant(self.valid_examples, dtype=tf.int32)

            # Ops and variables pinned to the CPU
            with tf.device('/cpu:0'):
                # Look up embeddings for inputs.
                embeddings = tf.Variable(tf.random_uniform([self.vocabulary_size, self.embedding_size], -1.0, 1.0))
                # according to embeddings, the 128-dimensional vector corresponding to the input word(train inputs) was extracted
                embed = tf.nn.embedding_lookup(embeddings, train_inputs)

                # Construct the variables for the NCE loss
                nce_weights = tf.Variable(tf.truncated_normal([self.vocabulary_size, self.embedding_size], stddev=1.0 / math.sqrt(self.embedding_size)))
                nce_biases = tf.Variable(tf.zeros([self.vocabulary_size]))
            # Compute the average NCE loss for the batch.
            # tf.nce_loss automatically draws a new sample of the negative labels each
            # time we evaluate the loss.
            loss = tf.reduce_mean(
                tf.nn.nce_loss(weights=nce_weights,
                             biases=nce_biases,
                             labels=train_labels,
                             inputs=embed,
                             num_sampled=self.num_sampled,
                             num_classes=self.vocabulary_size))
            
            # Construct the SGD optimizer using a learning rate of 1.0.
            optimizer = tf.train.GradientDescentOptimizer(1.0).minimize(loss)

            # Compute the cosine similarity between minibatch examples and all embeddings.
            norm = tf.sqrt(tf.reduce_sum(tf.square(embeddings), 1, keep_dims=True))
            normalized_embeddings = embeddings / norm
            valid_embeddings = tf.nn.embedding_lookup(normalized_embeddings, valid_dataset)
            similarity = tf.matmul(valid_embeddings, normalized_embeddings, transpose_b=True)

            # Add variable initializer.
            init = tf.global_variables_initializer()
            
        with tf.Session(graph = self.graph) as session:
            init.run()
            average_loss = 0
            
            for step in xrange(self.num_steps):
                batch_inputs, batch_labels = self.generate_batch(data)
                feed_dict = {train_inputs: batch_inputs, train_labels: batch_labels}

                # we perform one update step by evaluating the optimizer op (including it
                # in the list of returned values for session.run()
                _, loss_val = session.run([optimizer, loss], feed_dict=feed_dict)
                average_loss += loss_val
                
                if step % 2000 == 0:
                    if step > 0:
                        average_loss /= 2000
                    # the average loss is an estimate of the loss over the last 2000 batches.
                    print('Average loss at step ', step, ': ', average_loss)
                    average_loss = 0
                
                # output the most similar eight words to the screen
                if step % 10000 == 0:
                    sim = similarity.eval()
                    for i in xrange(self.valid_size):
                        valid_word = reverse_dictionary[self.valid_examples[i]]
                        top_k = 8  # number of nearest neighbors
                        nearest = (-sim[i, :]).argsort()[1:top_k + 1]
                        log_str = 'Nearest to %s:' % valid_word
                        for k in xrange(top_k):
                            close_word = reverse_dictionary[nearest[k]]
                            log_str = '%s %s,' % (log_str, close_word)
                        print(log_str)
                        
            self.final_embeddings = normalized_embeddings.eval()
            
    # visualize the embeddings
    def plot_with_labels(self, low_dim_embs, labels, filename='tsne.png'):
        assert low_dim_embs.shape[0] >= len(labels), 'More labels than embeddings'
        plt.figure(figsize=(18, 18))  # in inches
        for i, label in enumerate(labels):
            x, y = low_dim_embs[i, :]
            plt.scatter(x, y)
            plt.annotate(label,
                            xy=(x, y),
                            xytext=(5, 2),
                            textcoords='offset points',
                            ha='right',
                            va='bottom')
        plt.show()
        #plt.savefig(filename)
```


```
if __name__ == "__main__":
    try:
        bpe = BasicPatternEmbedding()
        filename = bpe.maybe_download('text8.zip',31344016)
        vocabulary = bpe.read_data(filename)
        print('Data size', len(vocabulary))
        print ('vocabulary:', vocabulary[:10])
        
        data, count, dictionary, reverse_dictionary = bpe.build_dataset(vocabulary)
        del vocabulary  # Hint to reduce memory.
        print('Most common words (+UNK)', count[:5])
        print('Sample data', data[:10], [reverse_dictionary[i] for i in data[:10]])
        
        batch, labels = bpe.generate_batch(data)
        for i in range(8):
            print(batch[i], reverse_dictionary[batch[i]], '->', labels[i, 0], reverse_dictionary[labels[i, 0]])
        print (dictionary['a'], dictionary['as'], dictionary['term'])
        
        bpe.train(data, reverse_dictionary)
        
        tsne = TSNE(perplexity=30, n_components=2, init='pca', n_iter=5000, method='exact')
        plot_only = 300
        low_dim_embs = tsne.fit_transform(bpe.final_embeddings[:plot_only, :])

        labels = [reverse_dictionary[i] for i in xrange(plot_only)]
        bpe.plot_with_labels(low_dim_embs, labels)

    except ImportError:
        print('Please install sklearn, matplotlib, and scipy to show embeddings.')
```

    Found and verified text8.zip
    Data size 17005207
    vocabulary: ['anarchism', 'originated', 'as', 'a', 'term', 'of', 'abuse', 'first', 'used', 'against']
    Most common words (+UNK) [['UNK', 2735459], ('the', 1061396), ('of', 593677), ('and', 416629), ('one', 411764)]
    Sample data [0, 3081, 12, 6, 195, 2, 3134, 46, 59, 156] ['UNK', 'originated', 'as', 'a', 'term', 'of', 'abuse', 'first', 'used', 'against']
    3081 originated -> 12 as
    3081 originated -> 0 UNK
    12 as -> 6 a
    12 as -> 3081 originated
    6 a -> 195 term
    6 a -> 12 as
    195 term -> 2 of
    195 term -> 6 a
    6 12 195
    Average loss at step  0 :  185.77481079101562
    Nearest to it: confidence, doesn, theatre, came, gulf, cultural, sites, corps,
    Nearest to use: buried, grave, observation, dust, batman, security, hungarian, opens,
    Nearest to at: warrior, total, rivers, yards, reaction, extinction, exclusively, eu,
    Nearest to if: emergency, present, developing, dates, life, for, pennsylvania, genesis,
    Nearest to between: grant, execution, generally, power, official, interpreted, hiv, binary,
    Nearest to people: unlikely, mainly, prussian, dedicated, shot, spending, dangerous, pick,
    Nearest to states: forward, racing, begins, printed, follow, vacuum, study, mythology,
    Nearest to by: rulers, protestant, marvel, republic, zero, letters, researchers, amiga,
    Nearest to american: hit, stores, managed, practiced, intermediate, retrieved, moreover, unique,
    Nearest to world: leadership, decay, culture, false, vii, et, dialogue, gave,
    Nearest to but: denominations, passing, according, germans, medical, emperors, working, grant,
    Nearest to an: removed, marxist, experts, ac, eugene, bones, tree, ne,
    Nearest to were: coat, facing, grammar, storage, teach, covering, solomon, circuit,
    Nearest to to: plant, supporting, pay, pp, shell, problem, acids, post,
    Nearest to be: judah, photo, films, both, senate, woman, villages, eating,
    Nearest to used: legislative, hero, private, organ, spaces, vice, top, trivia,
    Average loss at step  2000 :  22.257665908694268
    Average loss at step  4000 :  5.249317247629166
    Average loss at step  6000 :  4.652066127896309
    Average loss at step  8000 :  4.529780765414238
    Average loss at step  10000 :  4.432040006399155
    Nearest to it: he, came, votes, matters, doesn, whole, confidence, continues,
    Nearest to use: alien, buried, security, hungarian, grave, dust, batman, amount,
    Nearest to at: in, killed, appearance, extinction, mathbf, rivers, eu, pronunciation,
    Nearest to if: life, molecules, emergency, dates, present, pennsylvania, for, rates,
    Nearest to between: eight, execution, vs, of, hiv, grant, official, documentary,
    Nearest to people: UNK, mainly, dedicated, selection, unlikely, shot, fact, dangerous,
    Nearest to states: forward, racing, cover, arithmetic, study, vacuum, vs, begins,
    Nearest to by: and, as, in, infant, co, manufacturer, with, campaign,
    Nearest to american: hit, importance, austin, entry, depending, retrieved, vs, intermediate,
    Nearest to world: culture, leadership, UNK, false, mathbf, skills, et, titled,
    Nearest to but: and, medical, working, was, connecticut, vs, europeans, denominations,
    Nearest to an: the, ac, plant, challenge, experts, necessary, lake, marxist,
    Nearest to were: jpg, are, facing, covering, manual, circuit, opposite, test,
    Nearest to to: ends, and, plant, in, office, into, supporting, agave,
    Nearest to be: iso, shorter, judah, self, painter, also, dependent, assistance,
    Nearest to used: opposition, hero, private, illinois, legislative, regime, breaking, repeated,

![](https://raw.githubusercontent.com/imonce/imgs/master/20191021100559.png)

# 相关函数说明

## Tensor.eval()

.eval() 其实就是tf.Tensor的Session.run() 的另外一种写法，但两者有差别

1. eval(): 将字符串string对象转化为有效的表达式参与求值运算返回计算结果
2. eval()也是启动计算的一种方式。基于Tensorflow的基本原理，首先需要定义图，然后计算图，其中计算图的函数常见的有run()函数，如sess.run()。同样eval()也是此类函数，
3. 要注意的是，eval()只能用于tf.Tensor类对象，也就是有输出的Operation，写作Tensor.eval()。对于没有输出的Operation, 可以用.run()或者Session.run()；Session.run()没有这个限制。

## np.argsort()

argsort函数返回的是数组值从小到大的索引值

```
>>> x = np.array([3, 1, 2])
>>> np.argsort(x)
array([1, 2, 0])
```

## tf.reduce_sum()

reduce_sum( ) 是求和函数，在 tensorflow 里面，计算的都是 tensor，可以通过调整 axis =0,1 的维度来控制求和维度。

、、、
>>> x = tf.constant([[1,1,1],[1,1,1]])
>>> tf.reduce_sum(x)
6
>>> tf.reduce_sum(x, 0)
[2,2,2]
>>> tf.reduce_sum(x, 1)
[3,3]
>>> tf.reduce_sum(x, 1, keepdims=True)
[[3],[3]]
>>> tf.reduce_sum(x, [0,1])
6
、、、

## tf.nn.nce_loss()

假设nce_loss之前的输入数据是K维的，一共有N个类，那么

weight.shape = (N, K)

bias.shape = (N)

inputs.shape = (batch_size, K)

labels.shape = (batch_size, num_true)

num_true : 实际的正样本个数

num_sampled: 采样出多少个负样本

num_classes = N

sampled_values: 采样出的负样本，如果是None，就会用不同的sampler去采样。待会儿说sampler是什么。

remove_accidental_hits: 如果采样时不小心采样到的负样本刚好是正样本，要不要干掉

partition_strategy：对weights进行embedding_lookup时并行查表时的策略。TF的embeding_lookup是在CPU里实现的，这里需要考虑多线程查表时的锁的问题

nce_loss的实现逻辑如下：

_compute_sampled_logits: 通过这个函数计算出正样本和采样出的负样本对应的output和label

sigmoid_cross_entropy_with_logits: 通过 sigmoid cross entropy来计算output和label的loss，从而进行反向传播。这个函数把最后的问题转化为了num_sampled+num_real个两类分类问题，然后每个分类问题用了交叉熵的损伤函数，也就是logistic regression常用的损失函数。TF里还提供了一个softmax_cross_entropy_with_logits的函数，和这个有所区别。

在训练过程中，作为input的embed也会被自动更新

## tf.nn.embedding_lookup()

```
# Signature:
tf.nn.embedding_lookup(params, ids, partition_strategy='mod', name=None, validate_indices=True, max_norm=None)
# Docstring:
# Looks up `ids` in a list of embedding tensors.
```

是根据 ids 中的id，寻找 params 中的第id行。比如 ids=[1,3,5]，则找出params中第1，3，5行，组成一个tensor返回。

embedding_lookup不是简单的查表，params 对应的向量是可以训练的，训练参数个数应该是 feature_num * embedding_size，即前文表述的embedding层权重矩阵，就是说 lookup 的是一种全连接层。

partition_strategy 为张量编号方式，在张量存在多维时起作用，编号的方式有两种，”mod”（默认） 和 “div”。

假设：一共有三个tensor [a,b,c] 作为params 参数，所有tensor的第 0 维上一共有 10 个项目（id 0 ~ 9）。

“mod” : (id) mod len(params) 得到多少就把 id 分到第几个tensor里面

- a 依次分到id： 0 3 6 9
- b 依次分到id： 1 4 7
- c 依次分到id： 2 5 8

“div” : (id) div len(params) 可以理解为依次排序，但是这两种切分方式在无法均匀切分的情况下都是将前(max_id+1)%len(params)个 partition 多分配一个元素.

- a 依次分到id： 0 1 2 3
- b 依次分到id： 4 5 6
- c 依次分到id： 7 8 9

## tf.SparseTensor()

构造稀疏向量矩阵，每一行为一个样本

SparseTensor(indices, values, dense_shape)

```
SparseTensor(indices=[[0, 0], [1, 2]], values=[1, 2], dense_shape=[3, 4])

# represents the dense tensor

[[1, 0, 0, 0]
 [0, 0, 2, 0]
 [0, 0, 0, 0]]
```

> reference:
> https://blog.csdn.net/qoopqpqp/article/details/76037334
> https://segmentfault.com/a/1190000015287066?utm_source=tag-newest
> https://blog.csdn.net/u012193416/article/details/83349138
> https://blog.csdn.net/qq_36092251/article/details/79684721
> https://gshtime.github.io/2018/06/01/tensorflow-embedding-lookup-sparse/