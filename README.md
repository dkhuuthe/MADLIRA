# MADLIRA
Malware detection using learning and information retrieval for Android

## Overview
MADLIRA is a tool for Android malware detection. It consists in two components: TFIDF component and SVM learning component. In gerneral, it takes an input a set of malwares and benwares and then  extracts the malicious behaviors (TFIDF component) or computes training model (SVM classifier). Then, it uses this knowledge to detect malicious behaviors in the Android application.

## Insalling
Download file [MADLIRA.7z](https://lipn.univ-paris13.fr/~dam/tool/androidTool/MADLIRA_.7z) and decompress it.

### Installed Data:
+ *MADLIRA.jar* is the main application.
+ *noAPI.txt*  declares the prefix of APIs.
+ *family.txt* lists malwares by family.
+ Folder *TrainData* contains the training configuration and training model.
+ Folder *Samples*  contains sample data.
+ Folder *TempData* contains data for kernel computation.


## Functionality
This tool have two main components: TFIDF component and SVM component.
### TFIDF component
```
Command: MADLIRA TFIDF
```
For this component, there are two functions: the training function (Malicious behavior extraction) and the test function (Malicious behavior detection)

#### Malicious behavior extraction
+ Collect benign applications and malicious applications and oput them in folders named benginAPKFolder and maliciousApkFolder, respectively.
+ Prepare training data and pack them in two files named benignPack and maliciousPack by using the command:
```
MADLIRA TFIDF packAPK -PB benignApkFolder -B benignPack -PM maliciousApkFolder -M maliciousPack
```
+ Extracting malicious behaviors from two packed files (benignPack and maliciousPack) by using the command:
```
MADLIRA TFIDF train -B benignPack -M maliciousPack
```
#### Malicious behavior detection
+ Collect new applications and put them in a folder named checkApk.
+ Detect malicious behaviors of applications in the folder checkApk by using the command:
```
MADLIRA TFIDF check -S checkApk
```

**Command:**
```
MADLIRA TFIDF train <Options>
        Compute the malicious specifications for given training data.
                -B <filename>: the archive file contains all graphs of training benwares.
                -M <filename>: the archive file contains all categories of training malwares.

MADLIRA TFIDF check <Options>
        Check malicious behaviors in the given applications in a folder.
                -S <folder>: the folder contains all applications (apk files).

MADLIRA TFIDF test <Options>
        Test the classifier for a given test data.
                -S <folder>: the folder contains all graphs for testing.

MADLIRA TFIDF clear
        Clean all training data.

MADLIRA TFIDF install
        Clean old training data and install a new data for training.
                -B <filename>: the archive file contains all graphs of training benwares.
                -M <filename>: the archive file contains all categories of training malwares.

```
#### Examples:
**Training new data:**
+ First collect training applications (APK files) and store them in folders named MalApkFolder and BenApkFolder.
+ Pack training applications into archive files named MalPack and BenPack by using this command:
```
MADLIRA TFIDF packAPK -PB BenApkFolder -B BenPack -PM MalApkFolder -M MalPack
```
+ Clean old training data:
```
MADLIRA TFIDF clear
```
+ Compute the malicious graphs from the training packs (BenPack and MalPack)
```
MADLIRA TFIDF train -B BenPack -M MalPack
```
**Checking new applications:**
+ put these applications in a folder named checkApk and use this command:
```
MADLIRA TFIDF check -S checkApk
```
Output:
![output](https://github.com/dkhuuthe/MalDet/raw/path/images/testSamples.PNG)

### SVM component
```
Command: MADLIRA SVM
```
For this component, there are two functions: the training function and the test function.
#### Training phase
+ Collect benign applications  in a folder named benignApkFolder and malicious applications in a folder named maliciousApkFolder.
+ Prepare training data by using the commands:
```
MADLIRA SVM packAPK -PB benignApkFolder -B benignPack -PM maliciousApkFolder -M maliciousPack
```
+ Compute the training model by this command:
```
MADLIRA SVM train -B benignPack -M maliciousPack
```
#### Malicious behavior detection
+ Collect new  applications and put  them in a folder named checkApk
+ Detect malicious behaviors of applications in the folder checkApk by using the command:
```
MADLIRA SVM check -S checkApk
```
**Command:**
```
MADLIRA SVM train <Options>
        Compute the classifier for given training data.
                -T <T>: max length of the common walks (default value = 3).
                -l <lambda>: lambda value to control the importance of length of walks (default value = 0.4).
                -B <filename>: the archive file contains all graphs of training benwares.
                -M <filename>: the archive file contains all graphs of training malwares.

MADLIRA SVM check <Options>
        Check malicious behaviors in the applications in a folder.
                -S <foldername>: the folder contains all apk files.

MADLIRA SVM test <Options>
        Test the classifier for given graph data.
                -S <foldername>: the folder contains all graphs of test data.
                -n <n>: the number of test samples.

MADLIRA SVM clear
        Clean all training data.
```

### Packages:
This tool uses the following packages:
+ apktool-2.2.1 (https://ibotpeaches.github.io/Apktool/)
+ ojalgo-41.0.0 (https://github.com/optimatika/ojAlgo)
+ libsvm (http://www.csie.ntu.edu.tw/~cjlin/libsvm/)

### References
+ Khanh Huu The Dam and Tayssir Touili. Extracting Android Malicious Behaviors. In Proceedings of ForSE 2017
+ Khanh Huu The Dam and Tayssir Touili. Learn Android malware. In Proceedings of IWSMA@ARES 2017
