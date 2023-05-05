Download Link: https://assignmentchef.com/product/solved-cse545-assignment-3
<br>
<strong><u>Overview</u></strong>

<strong><u>Hadoop-Style Cluster</u></strong>

<strong><u>Data</u></strong>

<strong><u>Part I: Word Count and Hypothesis Tes ng (50 Points)</u></strong>

<strong><u>Part II: Recommenda on System (40 Points)</u></strong>

<strong>Overview</strong>

<table width="740">

 <tbody>

  <tr>

   <td width="148"> </td>

   <td width="443">Goals.●             Gain experience with a live hadoop-style (hdfs,  spark) cluster. ● Implement hypothesis testing with multi-test correction at scale.●             Implement a basic collaborative ltering recommendation system.●             Gain experience navigating a cloud console to spin up a cluster. ● Work with moderately large data.General Requirements. You must use Python version 3.6 or later, Spark2.4.4 or later.  You will use a cluster for this assignment that comes with Spark already, but you may start development using Spark on your own or non-cluster machines.  Everything between input and output must occur within Spark RDDs.Python Libraries. The only data science, machine learning, or statistics libraries that you may import are those that are listed in this assignment. Of these libraries, you may not use any subcomponents that speci cally implement a concept which the instructions indicate you should implement (e.g. hypothesis testing, linear regression,collaborative       ltering).  Other Python default, non-data science libraries(e.g. sys, basic IO, re, random,csv) may be used — ask if unsure. All provided method names and classes must be used as provided with the same parameters. However, you may also use additional methods to keep your code clean.  The intention is for you to implement the algorithms we have gone over and problem solve in order to best understand the concepts of this course and their practical application. Current allowed data science-related libraries include:numpy as np //for matrices and matrix algebra; not ok for calling linear regression</td>

   <td width="148"> </td>

  </tr>

  <tr>

   <td width="148"> </td>

   <td width="443">randomPublished by <a href="https://docs.google.com/">Google Drive</a> – <a href="https://docs.google.com/u/1/abuse?id=AKkXjowCtEi-DcMUcHnznUkDdC0IvbGI-bE4YM2CdZPW-DSInm4BsV6ktNBQjxTvSZcW_4F1TLWl3twY9LrH-Ic:0">Report Abuse </a>scipy.stats //for distributions</td>

   <td width="148"> </td>

  </tr>

  <tr>

   <td width="148">CSE545 Sp20 –</td>

   <td width="443">Assignment 3 DescriptionSubmission.<sub>blackboard:</sub> Please attach the follow les to the assignment submission inUpdated auto<sub>minutes</sub></td>

   <td width="148">matically every 5</td>

  </tr>

  <tr>

   <td width="148"> </td>

   <td width="443">1.          a3_cluster_screenshot_[lastname]_[id].png — console screenshot of your running cluster2.          a3_p1_[lastname]_[id].py — your python Spark code for part 1.3.          a3_p2_[lastname]_[id].py — your python Spark code for part 2.(do not include the brackets [] in your file name — those are placeholders for your name and id number)Academic Integrity.  Copying chunks of code or problem solving answers from other students, online or other resources is prohibited. You are responsible for both (1) not copying others work, and (2) making sure your work is not accessible to others (now or anytime in the future; exceptions may be made for private sharing with potential employers if instructor is contacted). Assignments will be extensively checked for copying of others’ work. Problem solving solutions are expected to be original using concepts discussed in the book, class, or supplemental materials but not using any direct code or answers. Please see the syllabus for additional policies.</td>

   <td width="148"> </td>

  </tr>

 </tbody>

</table>

<table width="740">

 <tbody>

  <tr>

   <td width="148"> </td>

   <td width="443">Initially, you will have access to a modest class cluster if you wish to test code in such an environment. In time, you will receive information to spin up your own cluster.<strong><u>To access the class cluster:  </u></strong>●             Submit your <a href="https://www.google.com/url?q=https://git-scm.com/book/en/v2/Git-on-the-Server-Generating-Your-SSH-Public-Key&amp;sa=D&amp;ust=1590797117373000"><em>publi</em></a><a href="https://www.google.com/url?q=https://git-scm.com/book/en/v2/Git-on-the-Server-Generating-Your-SSH-Public-Key&amp;sa=D&amp;ust=1590797117374000"><em>c</em></a><a href="https://www.google.com/url?q=https://git-scm.com/book/en/v2/Git-on-the-Server-Generating-Your-SSH-Public-Key&amp;sa=D&amp;ust=1590797117374000"> ssh rsa key</a> to <a href="https://www.google.com/url?q=https://docs.google.com/forms/d/e/1FAIpQLSc0E2-d-KSxSIQucaoBLdW92npcq9X5pGisz93hv3uQT8RK6A/viewform?usp%3Dsf_link&amp;sa=D&amp;ust=1590797117374000">this form</a><a href="https://www.google.com/url?q=https://docs.google.com/forms/d/e/1FAIpQLSc0E2-d-KSxSIQucaoBLdW92npcq9X5pGisz93hv3uQT8RK6A/viewform?usp%3Dsf_link&amp;sa=D&amp;ust=1590797117374000">.</a><strong>(May take up to 36 hours to enable access)</strong>●             Once a TA acknowledges that you have been added to the server try to ssh ina.            User name: (TA will provide)b.            Address:  <strong>&lt;ADDRESS&gt;</strong>■ port: 22(use your <em>private </em>(id_rsa or *.ppk) key on your end)c.             Set spark environment variable to python3 (using nano, vi, or emacs)■ add “exportPYSPARK_PYTHON=python3” to .bashrc■ run “source .bashrc”d.            Test that spark shell works for you:$ pyspark Python 3.6.10 ….… &gt;&gt;&gt; rdd =sc.textFile(‘hdfs:/data/Software_5.json.gz’)              &gt;&gt;&gt; rdd.take(2)</td>

   <td width="148"> </td>

  </tr>

  <tr>

   <td width="148"> </td>

   <td width="443">                  [‘{“overall”: 4.0, “verified”: false,“reviewTime”: “10 …Published by <a href="https://docs.google.com/">Google Drive</a> – <a href="https://docs.google.com/u/1/abuse?id=AKkXjowCtEi-DcMUcHnznUkDdC0IvbGI-bE4YM2CdZPW-DSInm4BsV6ktNBQjxTvSZcW_4F1TLWl3twY9LrH-Ic:0">Report Abuse</a></td>

   <td width="148"> </td>

  </tr>

 </tbody>

</table>

<strong>Hadoop-Style Cluster (10 points)</strong>




<table width="740">

 <tbody>

  <tr>

   <td width="148">CSE545 Sp20 –</td>

   <td width="443">Assignment 3 Description<strong>Spin up your own cluster (10 Points)          </strong>Updated auto<a href="https://www.google.com/url?q=https://piazza.com/class/k5u5mhd7g3t2ur?cid%3D234&amp;sa=D&amp;ust=1590797117377000">minutes </a>● Sign up for Google Cloud credits according to <a href="https://www.google.com/url?q=https://piazza.com/class/k5u5mhd7g3t2ur?cid%3D234&amp;sa=D&amp;ust=1590797117377000">this post</a><a href="https://www.google.com/url?q=https://piazza.com/class/k5u5mhd7g3t2ur?cid%3D234&amp;sa=D&amp;ust=1590797117377000">.</a></td>

   <td width="148">matically every 5</td>

  </tr>

  <tr>

   <td width="148"> </td>

   <td width="443">● Spin up a cluster and take screen shot.<a href="https://www.google.com/url?q=https://www.youtube.com/watch?v%3D6DD-vBdJJxk&amp;sa=D&amp;ust=1590797117377000">Follow this tutorial: </a><a href="https://www.google.com/url?q=https://www.youtube.com/watch?v%3D6DD-vBdJJxk&amp;sa=D&amp;ust=1590797117377000">https://www.youtube.com/watch?v=6DDvBdJJxk</a><a href="https://www.google.com/url?q=https://www.youtube.com/watch?v%3D6DD-vBdJJxk&amp;sa=D&amp;ust=1590797117377000"> .</a>

    <table width="427">

     <tbody>

      <tr>

       <td width="427">Name                         [you decide]Region                       us-east1Zone                           us-east1-cAutoscaling              OffScheduled deletion    OffEnhanced exibility mode    Off<strong>Master node             Standard (1 master, N workers)</strong><strong>Machine type            e2-standard-2</strong>Number of GPUs      0Primary disk type     pd-standard<strong>Primary disk size      64GB</strong><strong>Worker nodes            2</strong><strong>Machine type            e2-highmem-4</strong>Number of GPUs       0Primary disk type       pd-standard<strong>Primary disk size       32GB</strong>Local SSDs                  0Preemptible worker nodes   0Cloud Storage staging bucket   —Subnetwork            defaultNetwork tags         NoneInternal IP only       No<strong>Image version        1.4.26-debian9</strong></td>

      </tr>

     </tbody>

    </table>For now, use the following con guration (You can use any “east” region and zone as long as the zone matches the region):Image Version:                                                           1.4 (Debian 9, Hadoop 2.9,Spark 2.4)(con guring a jupyter notebook is optional;  note: the tutorial is 1.5 years old; some things look slightly di erent.)It may be useful to get Google SDK for your local machine:<a href="https://www.google.com/url?q=https://cloud.google.com/sdk/docs/&amp;sa=D&amp;ust=1590797117382000">https://cloud.google.com/sdk/docs/</a>Alternative setups that have the same total number of VPUs (8) and total memory (64GB) are            ne.[Take a screenshot of <u>console.cloud.google.com/dataproc/cluster</u>s to show your cluster “running”. ]● Test the cluster.Set pyspark to use python3:<strong>        </strong>add “export PYSPARK_PYTHON=python3” to .bashrc                 (use “nano .bashrc” or install your preferred editor)run “source .bashrc”Launch pyspark: “pyspark” and try a few things:</td>

   <td width="148"> </td>

  </tr>

  <tr>

   <td width="148"> </td>

   <td width="443">sc._jsc.sc().getExecutorMemoryStatus().size() #returns the number of <sub>nodes </sub>Published by <a href="https://docs.google.com/">Google Drive</a> – <a href="https://docs.google.com/u/1/abuse?id=AKkXjowCtEi-DcMUcHnznUkDdC0IvbGI-bE4YM2CdZPW-DSInm4BsV6ktNBQjxTvSZcW_4F1TLWl3twY9LrH-Ic:0">Report Abuse</a></td>

   <td width="148"> </td>

  </tr>

 </tbody>

</table>




<strong>Data</strong>

<table width="740">

 <tbody>

  <tr>

   <td width="148"> </td>

   <td width="443">Both parts of this assignment will work with the same datasets of</td>

   <td width="148"> </td>

  </tr>

  <tr>

   <td width="148">CSE545 Sp20 –</td>

   <td width="443">Assignment 3 DescriptionAmazon reviews. The data  comes in both a small form (forUpdated auto<sub>minutes </sub>developing your code) and a larger form (for testing your code):</td>

   <td width="148">matically every 5</td>

  </tr>

  <tr>

   <td width="148"> </td>

   <td width="443"><a href="https://www.google.com/url?q=http://deepyeti.ucsd.edu/jianmo/amazon/categoryFilesSmall/Software_5.json.gz&amp;sa=D&amp;ust=1590797117389000">Software_5.json.gz</a> — small data — software reviewsAvailable in class cluster under hdfs:/data/Software_5.json.gz N = 12,805 Reviews<a href="https://www.google.com/url?q=http://deepyeti.ucsd.edu/jianmo/amazon/categoryFilesSmall/Books_5.json.gz&amp;sa=D&amp;ust=1590797117390000">Books_5.json.gz</a> — large data — book reviews (warning may take up to an hour to download). Available in class cluster under hdfs:/data/Books_5.json.gz N = 27,164,983 ReviewsThe format of the file is JSON, and the following are the fields that will be relevant for this assignment (all others may be filtered out during your first steps):{“overall”: #rating score from 1 to 5,“reviewerID”: #string id of the reviewer (e.g.A2SUAM1J3GNN3B),“asin”: #long integer id of the product (e.g. e.g. 0000013714),“reviewText”: #string of the review,“summary”: #summary of the text,“verified”: #true or false: whether the purchase was verified (assume false if not present)}Original data is from <a href="https://www.google.com/url?q=https://nijianmo.github.io/amazon/index.html&amp;sa=D&amp;ust=1590797117392000">(Ni, 2018)</a>.</td>

   <td width="148"> </td>

  </tr>

 </tbody>

</table>

<strong>Part I: Word Count and Hypothesis Testing (50 Points)</strong>

<table width="740">

 <tbody>

  <tr>

   <td width="148"> </td>

   <td width="443">Here, you will attempt to find significant associations between words and ratings by using multi-test corrected hypothesis testing.<strong>Filename:</strong> a3_p1_&lt;lastname&gt;_&lt;id&gt;.py<strong>Input:</strong> Your code should take <strong>one</strong> command line parameter for the review dataset location.Example: spark-submit a3_P1_LAST_ID.py‘hdfs:/data/Software_5.json.gz’<strong>Task Requirements: </strong>Your objective is to compute the correlationbetween each of the 1,000 most common words (case insensitive) across all reviews with the rating score for the reviews, controlling for whether the review was verified or not.First you must figure out which of all the possible words are the most common. You should consider anything matched by the following regular expression as a word:r'((?:[.,!?;”])|(?:(?:#|@)?[A-Za-z0-9_-]+ (?:'[a-z]{1,3})?))’Then, you must figure out how common each of the 1k words</td>

   <td width="148"> </td>

  </tr>

  <tr>

   <td width="148"> </td>

   <td width="443">occurs in each review. Record the relative frequency = (total Published by <a href="https://docs.google.com/">Google Drive</a> – <a href="https://docs.google.com/u/1/abuse?id=AKkXjowCtEi-DcMUcHnznUkDdC0IvbGI-bE4YM2CdZPW-DSInm4BsV6ktNBQjxTvSZcW_4F1TLWl3twY9LrH-Ic:0">Report Abuse</a></td>

   <td width="148"> </td>

  </tr>

  <tr>

   <td width="148"> </td>

   <td width="443">count of word) / (total number of words in review). Note most</td>

   <td width="148"> </td>

  </tr>

  <tr>

   <td width="148">CSE545 Sp20 –</td>

   <td width="443">Assignment 3 Descriptionwords will occur 0 times in most reviews.                                                                     Updated auto<sub>minutes</sub>Finally, compute the relationship. Each review represents an</td>

   <td width="148">matically every 5</td>

  </tr>

  <tr>

   <td width="148"> </td>

   <td width="443">observation, and each of the 1,000 words is essentially a hypothesis. Thus, you will have over 1k linear regressions to run representing 1,000 hypotheses to test. Further, you will need to run the tests without and using “verified” as a control (simply including it as an additional covariate in your linear regression as either 0 or 1).  You must use Spark such that each of these correlations (i.e. standardized linear regression) can be run in parallel — organize the data such that each record contains all data needed for a single word (i.e. all relative frequencies as well as corresponding ratings and verified indicators for each review), and then use a map to compute the correlation values for each.<em>You don’t have to worry about duplicate reviews for this one. Assume each review is a separate review.</em>You must choose how to handle the outcome and control data effectively. You must implement standardized multiple linear regression yourself — it is just a line or two of matrix operations (using Numpy is fine).  Finally, you must compute p values for each of the top 20 most positively and negatively correlated words and apply the Bonferroni multi-test correction.All together, your code should run in less than 8 minutes on the provided data. Your solution should be scalable, such that one simply needs to add more nodes to the cluster to handle 10x or 100x the data size.Other than the above, you are free to design what you feel is the most efficient and effective solution. Based on feedback, the instructor may add or modify restrictions (in minor ways) up to 3 days before the submission.  You are free to use broadcast or aggregator variables in ways that make sense and fit in memory -typically 1 row or 1 column by itself will fit in memory but not an entire matrix (at least for the larger dataset).<strong>Output: </strong>Your code should output four lists of results. For each word, output the triple: (“word”, beta_value, multi-test corrected(for 1000 hypothesis) p-value)1)  The top 20 word <em>positively</em> correlated with rating2)  The top 20 word <em>negatively</em> correlated  with rating3)  The top 20 words <em>positively </em>related to rating, <em>controlling</em><em>for verified</em>4)  The top 20 words <em>negatively </em>related to rating,<em>controlling for verified</em><em>Note: a Bonferroni-correct p-value adjusts the p-value according to the Bonferroni correction. We adjusted the alpha in class. Here, you are adjusting the p-value, so instead of dividing by the number of hypotheses, you will multiply to p-value.</em><strong><em>**Remember to save your code and delete/terminate your cluster when you’re not using it.**</em></strong></td>

   <td width="148"> </td>

  </tr>

 </tbody>

</table>

<strong>Part II: Recommendation System (40 Points)</strong>




<table width="740">

 <tbody>

  <tr>

   <td width="148"> </td>

   <td width="443">Here, you will create a collaborative filtering recommendation</td>

   <td width="148"> </td>

  </tr>

  <tr>

   <td width="148">CSE545 Sp20 –</td>

   <td width="443">Assignment 3 Descriptionsystem to suggest what users (i.e. reviewers) would rate a givenUpdated auto<sub>minutes </sub>item that they haven’t seen yet.</td>

   <td width="148">matically every 5</td>

  </tr>

  <tr>

   <td width="148"> </td>

   <td width="443"><strong>Filename:</strong> a3_p2_&lt;lastname&gt;_&lt;id&gt;.py(do not include the brackets &lt;&gt; in your file name)<strong>Input:</strong> Your code should take <strong>two</strong> command line parameters: (1) for the review dataset location, and (2) a list of product asins in python list format:Example:spark-submit a3_p2_LAST_ID.py‘hdfs:/data/Software_5.json.gz’ “[‘B00EZPXYP4’,‘B00CTTEKJW’]”<strong>Task Requirements: </strong>Your objective is to perform <strong>itemitem</strong> collaborative filtering over the provided products and ratings.Specifically,To prepare the system, you will first need to do some filtering:a.          Filter to only one rating per user per item by taking their most recent rating (or their last within the data file; as long as you have one rating per person it is fine)b.          From there, filter to items associated with at least 25 distinct usersc.          From there, filter to users associated with at least 5 distinct items<em>Option: If you have a particular RDD that has less than an order of 1k entries (i.e. a list of reviewerIDs or asins), at that point, it’s ok to collect them into a sc.Broadcast variable.</em>Then, you are ready to apply item-item collaborative filtering to predict missing values for the rows prescribed in the output. Use the following settings for your collaborative filtering:a.          Use 50 neighbors (or all possible neighbors if &lt; 50 have values) with the weighted average approach (weighted by similarity) described in class and the book.a. Do not include neighbors:i. with negative or zero similarity or ii. those having less than 2 columns (i.e. users) with ratings for whom the target row (i.e. the intersection of users_with_ratings for the two is &lt; 2; can check for this before checking similarity).b.          Within a target row, do not make predictions for columns (i.e. users) that do not have at least 2 neighbors with valuesc.          Only need to focus on the specified items (in practice, you wouldn’t store a completed utility matrix, rather this represents querying the recommendation system, given an item, for users that might be interested in the item).Remember to treat items as “rows” and users as “columns” where the goal is to rate one item based on its similarity to other items. Running from start (reading/filtering data) to finish (printing results should take less than 8 minutes on a cluster with &gt;= 8 vCPUs with 8GB per vCPU and multiple disks for reading the data from HDFS (In reality, such a system could assume that the data was already filtered as that wouldn’t need to happen per run but it is fine to happen per run here).</td>

   <td width="148"> </td>

  </tr>

  <tr>

   <td width="148"> </td>

   <td width="443">Published by <a href="https://docs.google.com/">Google Drive</a> – <a href="https://docs.google.com/u/1/abuse?id=AKkXjowCtEi-DcMUcHnznUkDdC0IvbGI-bE4YM2CdZPW-DSInm4BsV6ktNBQjxTvSZcW_4F1TLWl3twY9LrH-Ic:0">Report Abuse</a></td>

   <td width="148"> </td>

  </tr>

 </tbody>

</table>

<table width="740">

 <tbody>

  <tr>

   <td width="148"> </td>

   <td width="443"><strong>Output: </strong>Your code should output the following rows from the</td>

   <td width="148"> </td>

  </tr>

  <tr>

   <td width="148">CSE545 Sp20 –</td>

   <td width="443">Assignment 3 Descriptioncompleted utility matrix (including predictions for each user) forUpdated auto<sub>minutes </sub>the following products. Items for initial testing:</td>

   <td width="148">matically every 5</td>

  </tr>

  <tr>

   <td width="148"> </td>

   <td width="443">Software: B00EZPXYP4 (Norton), B00CTTEKJW (Amazon Music)Books(Tentative): 0008118922, 1469216051<strong><em>**Remember to save your code and delete/terminate your cluster when you’re not using it.**</em></strong></td>

   <td width="148"> </td>

  </tr>

 </tbody>

</table>


