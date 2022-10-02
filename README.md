# assignment2_vdantasdossantos
assignment2

/*

1. How do you classify a new tweet? Give a short description of the main steps.

First, the tweet is parsed into its individual words. The words are cleaned up a bit,
meaning that punctuation is removed, and suffixes are removed (stemming). For each word,
probability that the word appears in a positive/negative tweet is approximated by dividing
the number of times the word appeared in a positive tweet in the training set by the
number of positive tweets overall in the training set. These probabilities are then inserted
into Bayes' formula to get the probability that a tweet comprised of the words is positive.
The tweet is classified as positive if and only if the probability is more than 0.50.

2. How long did your code take for training and what is the time complexity of your training
    implementation (Big-Oh notation)? Explain why.

Running in debug mode, the training step (with 20000 tweets) took about 45 seconds. Running
in release mode, it took only about 2 seconds. For each tweet, the words in the tweet are
added to a vocabulary array. The array is maintained at all times in sorted order, so that
duplicate words can be identified by a binary search. In order to maintain sorted order,
each time a word is added to the vocabulary array, the array must be sorted. This sorting
step is the most complex step in the training. I used the std::sort method to accomplish
the sorting. Assuming std::sort performs in linearithmic time, based on the size of the
vocabulary array each time it is sorted, and assuming a sort operation is performed with
each new word added, the complexity of the training overall is (n^2)*log(n), where n is
the number of distinct words in all tweets in the training set.

Also, a performance enhancement was made to avoid sorting the entire vocabulary after each
new word. A second vocabulary array of size 500 was created. Each new word was initially
added to the smaller (size 500) array, and the smaller array was sorted with each new word
added to it. Once the smaller array became full, its contents were added to the larger
array (meant to contain the entire vocabulary), the larger array was sorted, and the smaller
array cleared out to accommodate the next 500 new words. This resulted in significant
reduction in runtime. However, the complexity is still (n^2)*log(n). Why? The smaller array
has size 500. So the complexity of sorting the smaller array can be regarded as constant.
The number of arrays of 500 that are needed is roughly n/500. And since the overall array
still has to be sorted after adding each chapter of 500 new words, the overall array is
sorted roughly (n/500) times. So the complexity is (n^2/500)*log(n), which is still
(n^2)*log(n).

3. How long did your code take for classification and what is the time complexity of your
classification implementation (Big-Oh notation)? Explain why.

Running in debug mode, the classification step (classifying 10000 tweets) took about 2
seconds. Running in release mode, it took less than 1 second. The complexity is n*log(n),
where n is the number of words in the all tweets in the testing set. This is due to having
to lookup each word in the array that was created during the training phase. Because the
words are stored in sorted order, a binary search (technically, an std::upper_bound search)
is used to look words up. The binary search is log(n) complexity. And since this is done
to each word, the overall classification step has complexity n*log(n).

4. How do you know that you use proper memory management? That is that you do not have a
memory leak?

When creating the DSSstring class, I was careful to avoid memory leaks by only performing
allocations and deallocations in a few places, namely in the reserve and clear function. 
The DSVector class was provided, so I assume that it is free of memory leaks. In the classifier, 
I utilized DSSstring and DSVector objects to store collections of words, rather than performing 
memory allocation/deallocation directly.

Furthermore, a Valgrind check was performed on the completed program, which reported no
memory leaks.

5. What was the most challenging part of the assignment?

The most challenging part was trying to improve the accuracy of the classifier. Initially,
using a simple formula for classifying tweets as positive/negative, and not using any
additional strategies such as stopword removal, word stemming, or Bayes' classification,
the accuracy was around 60%. Removing stopwords, adding stemming, and adding Bayes'
classification increased the accuracy to 67.9%.

*/
