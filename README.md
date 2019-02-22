# Lucene
# 1. Downloading/Buildng  
   More info on building here: https://github.com/apache/lucene-solr 
   some versions of elasticsearch use a different version of lucene. Build from a branch that supports your elasticsearch version
   ex: elastic 6.5 uses lucene 7.5
# 2. Files
## 2.1 The ee folder goes here: common/java/org/apache/lucene/analysis
![idea64_2019-02-21_15-49-26](https://user-images.githubusercontent.com/14272566/53173692-d85e6980-35f0-11e9-8a82-58cc131e3c5d.png)  
## 2.2 EstonianStemmer file goes here: common/java/org/tartarus/snowball/ext
![idea64_2019-02-21_15-55-40](https://user-images.githubusercontent.com/14272566/53173855-296e5d80-35f1-11e9-8ee3-57dd36789416.png)  
## 2.3 Add the ee folder and put the stopwords file here: common/resources/org/apache/lucene/analysis/ee
![idea64_2019-02-21_16-00-16](https://user-images.githubusercontent.com/14272566/53174162-ce893600-35f1-11e9-834a-088f63e89e54.png)  
## 2.4 Build lucene  
# 3. After building
 Locate the new analyzers jar file. This file will replace the default elasticsearch lucene analyzers jar file later on
![explorer_2019-02-21_16-08-04](https://user-images.githubusercontent.com/14272566/53174638-f88f2800-35f2-11e9-97aa-e87646cd5579.png)
# Elastic
# 1. Downloading/Building 
  More info on building here: https://github.com/elastic/elasticsearch dont forget to choose the correct branch (the one you decided to       base your lucene version off of)  
# 2. Adding files/Lines of code
## 2.1 Add the following code to the path displayed in the picture (middle part of the picture)  File: StemmerTokenFilterFactory
  ### import
  ```
  import org.tartarus.snowball.ext.EstonianStemmer;
  ```
  ### Code:
  ```
  else if ("estonian".equalsIgnoreCase(language)) {
    return new SnowballFilter(tokenStream, new EstonianStemmer());
  }
```
![2019-02-21_16-15-09](https://user-images.githubusercontent.com/14272566/53175048-df3aab80-35f3-11e9-8b2a-0ef31628664f.png)
## 2.2 Add the following code to the path displayed in the picture File: CommonAnalysisPlugin
  ### import
  ```
  import org.apache.lucene.analysis.ee.EstonianAnalyzer;
  ```
  ### 2.2.1 Code:
  ```
  analyzers.put("estonian", EstonianAnalyzerProvider::new);
  ```
![2019-02-21_16-27-57](https://user-images.githubusercontent.com/14272566/53175921-abf91c00-35f5-11e9-9e3f-0d9298451469.png)  
### 2.2.2 Code:
```
analyzers.add(new PreBuiltAnalyzerProviderFactory("estonian", CachingStrategy.LUCENE, EstonianAnalyzer::new));
```
![2019-02-21_16-27-52](https://user-images.githubusercontent.com/14272566/53175930-b1eefd00-35f5-11e9-8447-8c038b16a8ce.png)  
## 2.3 Add the following code to the path displayed in the picture File: Analysis
 ### import
  ```
  import org.apache.lucene.analysis.ee.EstonianAnalyzer;
  ```
  ### 2.2.2 Code:
```
namedStopWords.put("_estonian_", EstonianAnalyzer.getDefaultStopSet());
```
![2019-02-21_16-41-07](https://user-images.githubusercontent.com/14272566/53176816-82d98b00-35f7-11e9-82c2-110164e445a0.png)  

## 2.4 Add the EstonianAnalyzerProvider.java file
  ### The folder where the file goes  
  ![idea64_2019-02-21_16-36-31](https://user-images.githubusercontent.com/14272566/53176506-e0210c80-35f6-11e9-8e2d-5af3dcda3f3e.png)  
# 3. Change the default analyzers jar file thats downloaded by gradle
### Navigate to you gradle home folder, then navigate to the lucene-analyzers-common folder
### My path looks like this on windows: C:\Users\gm\\.gradle\caches\modules-2\files-2.1\org.apache.lucene\lucene-analyzers-common\7.5.0
![explorer_2019-02-21_16-49-39](https://user-images.githubusercontent.com/14272566/53177428-b49f2180-35f8-11e9-9ac3-a856f48ef2db.png)
### you will probably notice that there are 2 folders here, choose the one without the .sources jar file
### so in my case my full path would be e7fbdfc2297c3ff5d194d7bef95810504e52710e\lucene-analyzers-common-7.5.0.jar
### replace the jar file there with your custom lucene jar file (just in case change the name aswell)
### after replacing the jar file you should be able to build elasticsearch 
