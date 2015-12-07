Table of Content

==============

Part I   : TagSuggest Contents
Part II  : How To Compile TagSuggest
Part III : How To Run Cross-validation of THUTagSuggest
Part IV  : Input File Formats of Cross-validation
Part V   : Output File Formats of Cross-validation
Part VI  : How To Run UI && Testing a single passage of THUTagSuggest
Part VII : Input File Formats of UI && Testing a single passage
Part VIII: Output File Formats of UI && Testing a single passage
Part IX  : Literature
Part X   : Appendix
Part I: TagSuggest Contents

==============

Algorithms for Keyphrase Extraction and Social Tag Suggestion, including Cross-Validation Evaluator

build :　Working directory

GIZA++  mkcls  plain2snt.out : Essential to running WTM/WAM/WAM*

Part II: How To Compile TagSuggest
==============

Environment : java (support java 1.8.0)

ant : Start a terminal in the directory "TagSuggestion/", input command "ant" and then TagSuggest will be compiled; or you can choose the build.xml for "ant build" in eclipse.


Part III: How To Run Cross-validation of THUTagSuggest
==============

Test a single algorithm using Cross Evaluation : Specific commands can be found in file "command", a Training Class is corresponding to exactly a Suggesting Class. (Part VII Appendix show the correspondence between Training Class and Suggesting Class)

Parameters : --dataset="Input file path"
	      --trainer_class="Name of Training Class"
	      --suggester_class="Name of Suggesting Class"
	      --num_folds="Num the file divided into"
	      --config="Parameter1=Value1;Parameter2=Value2;..."(The parameters can be dataType, k, numtopics, niter, etc.. Parameters vary with models and no parameter means using default values)
	      --working_dir="Working directory"
	      --report="Path of report"

The default path of book.model and chinese_stop_word.txt is the same with the path of tagsuggest.jar. If you need change the path of them, you should add model="Path of both of them" to --config. "book.model" is used for maximum forward word-segmentation and "chinese_stop_word" records the stop words in chinese. e.g. --config="model=/home/meepo/TagSuggestion;dataType=DoubanPost;"

If you want to run SMT, you need another three files GIZA++, mkcls, and plain2snt.out. Their default path is the same with the path of tagsuggest.jar.

If you need change the path of them, you should add --giza_path="Path of them" as a parameter to the command.

The evaluation results on Douban Post Dataset (M_d=2)

| Algorithm | Precision | Recall | F1 |
|---|:---|:---|:---|
| WTM | 0.36828 | 0.45131 | 0.35410 |
| PMI | 0.31453	| 0.30884 | 0.26602 |
| KNN |	0.27914 | 0.26815 | 0.23365 |
| NaiveBayes |	0.23707 | 0.21926 | 0.19369 |
| TPR | 0.21436 |0.18183 | 0.16829 |
| NoiseTagLdaModel | 0.21206 | 0.17785 | 0.16538 |
| ExpandRankKE | 0.15213 | 0.16313 | 0.13441 |
| WAM | 0.15112 | 0.15756 | 0.13172 |
| TagLdaModel | 0.16191 | 0.13882 | 0.12801 |
| TFIDF | 0.12962 | 0.14525 | 0.11725 |
| WAMsample | 0.10288 | 0.10819 | 0.08945 |

The evaluation results on Keyword Post Dataset (M_d=2)

| Algorithm | Precision | Recall | F1 |
|---|:---|:---|:---|
| TFIDF | 0.21508 | 0.24943 | 0.22591 |
| ExpandRankKE | 0.20291 | 0.23672 | 0.21368 |
| NaiveBayes | 0.19729 | 0.24835 | 0.21267 |
| WAMsample | 0.19841 | 0.22931 | 0.20836 |
| WAMwithtitleInstead | 0.19721 | 0.22687 | 0.20676 |
| KNN | 0.19014 | 0.23957 | 0.20463 |
| TAM | 0.10893 | 0.1293 | 0.1153 |
| NoiseTagLdaModel | 0.07388 | 0.09015 | 0.07902 |
| TPR | 0.07315 | 0.08589 | 0.07732 |
| TagLdaModel | 0.04871 | 0.05879 | 0.05194 |


Part IV: Input File Formats of Cross-validation
==============

dataType=Post : {"id":"文章编号","content":"文章内容","extras":"","resourceKey":"","timestamp":0,"title":"文章标题","userId":"","tags":["标签1","标签2","标签3",...]}   (Focus on news)

Example : {"id":"1004838","content":"格非――1964年生于江苏省丹德县，毕业于华东师范大学中文系，获文学博士学位。1985年至2000年任教于华东师范大学，现为清华大学中文系教授。主要作品有《格非文集》、《欲望的旗帜》等。","extras":"","resourceKey":"","timestamp":0,"title":"敌人","userId":"","tags":["小说","荒诞","敌人","中国文学","格非"]}   (Demo file is post.dat/keypost.dat)

dataType=DoubanPost : {"doubanTags":{"标签1":权重1(在此可用豆瓣点赞数表示权重),"标签2":权重2,"标签3":权重3,...},"id":"文章编号","content":"文章内容","tags":[标签],"timestamp":0,"resourceKey":"","title":"文章标题","userId":"","extras":""}   (Focus on books)

Example : {"doubanTags":{"文化":5,"献给非哲学家的小哲学":6,"哲学":29,"法国":17},"id":"1000047","content":"全球化是必然趋势？仁者见仁，智者见智。有人惊呼：“狼来了！”有人担忧：“怎么办？”还有人在思考：“对世界来说，经济可以全球化，甚至货币也可以一体化，但文化则要鼓励多元化。”是的，只有本着文化多元化的精神，在尊重其他民族文化的同时，自身才能获得不断的发展与丰富。法国人做出了自己的探索与努力。今天，您面前的这一套“法兰西书库·睿哲系列”为您打开了一扇沟通的窗口。他山之石，可以攻玉。我们希望这样的对话可以走得越来越远。","tags":[],"timestamp":0,"resourceKey":"","title":"献给非哲学家的小哲学  睿哲系列","userId":"","extras":""}   (Demo file is bookPost70000.dat)


Part V: Output File Formats of Cross-validation
==============

The output is a text file,whose first seven columns are the major data.From the first column to the seventh column are these in order: the number of keywords that we ask the algorithm to output | precision rate(Pre.) | the variance of precision rate | recall rate(Rec.) | the variance of recall rate | Fmeans | the variance of Fmeans

we have that 1 / Fmeans = 1 / Pre. +1 / Rec.

Part VI: How To Run UI & Test a Single Passage with THUTagSuggest
==============

Command for training model : java -Xmx8G -jar tagsuggest.jar train.TrainWTM --input=/home/meepo/test/sampleui/bookPost70000.dat --output=/home/meepo/test/sample --config="dataType=DoubanPost;para=0.5;minwordfreq=10;mintagfreq=10;selfTrans=0.2;commonLimit=2" 

"input" is the train data's address.
"output" is where the model will be set.
"config"  is the config of the model.

 
Command for running UI : java -Xmx8G -jar tagsuggest.jar evaluation.GuiFrontEnd --model_path=/home/meepo/test/sampleui/  --algorithm=SMTTagSuggest --config="" --realtime=true 

"model_path" is the model's address.
"algorith" is the train class that we choose.
"config" is the config of thetrain class.

 
Test a single passage : java -Xmx8G -jar tagsuggest.jar evaluation.TestDemo --model_path=/home/meepo/test/sampleui/ --algorithm=SMTTagSuggest --config="" --article_path=/home/meepo/text --output_path=/home/meepo/tag 
  
"model_path" is the model's address.
"algorith" is the train class that we choose.
"config" is the config of thetraiclass.
 
 
The default path of book.model and chinese_stop_word.txt is the same with the path of tagsuggest.jar. If you need change the path of them, you should add model="Path of both of them" to --config. "book.model" is used for maximum forward word-segmentation and "chinese_stop_word" records the stop words in chinese. e.g. --config="model=/home/meepo/TagSuggestion;dataType=DoubanPost;"
 
If you want to run WTM/WAM/WAM*, you need another three files GIZA++, mkcls, and plain2snt.out. Their default path is the same with the path of tagsuggest.jar.If you need change the path of them, you should add --giza_path="Path of them" as a parameter to the command. 


Part VII: Input File Formats of UI & Testing a Single Passage
==============

In the UI interface,you can input text directly.
And when test a individual text file,the text file must contains two lines:the first line is the title of the article and the second line is the content of the article.

Part VIII: Output File Formats of UI & Testing a Single Passage
==============

In the UI interface,our program will show the keywords to the screen directly.
And when test a individual text file,the program will give back a text file with ten keywords that the algorithm forecast and their corresponding weights.

Part IX: Literature
==============


Part X: Appendix
==============

Correspondence between Training Class and Suggesting Class

| Training Class | Suggesting Class |
|---|:---|
| TrainExpandRank | ExpandRankKE |
| TrainKnn | KnnTagSuggest |
| TrainNaiveBayes | NaiveBayesTagSuggest |
| TrainNoiseTagLdaModel | NoiseTagLadaTagSuggest |
| TrainPMI | PMITagsuggest |
| TrainTagLdaModel | TagLdaTagSuggest |
| TrainTAM | TAMTagSuggest |
| TrainTFIDF | TFIDFTagSuggest |
| TrainTPR | TPRTagSuggest |
| TrainWAM | SMTTagSuggest |
| TrainWAMsample | SMTTagSuggest |
| TrainWAMWithtitleInstead | SMTTagSuggest | 
| TrainWTM | SMTTagSuggest |
