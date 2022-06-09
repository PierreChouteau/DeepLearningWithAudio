# DDSP Training in AzureVM

Log in to  https://labs.azure.com
(see the  [login instructions](https://github.com/SopiMlab/DeepLearningWithAudio/blob/master/00_introduction/))

c/p the command line below into your ternimnal window to go to the dlwa directory

```
cd /data/dome5132fileshare/DeepLearningWithAudio/utilities/dlwa
```


## Transfering your dataset to the virtual machine

You can transfer your files from your own PC to the vm following the below command line structure. Open a new terminal window make sure that you are in your own computer/laptop directory

* Transfering a folder
```
scp -P 63635 -r input_folder e5132-admin@ml-lab-00cec95c-0f8d-40ef-96bb-8837822e93b6.westeurope.cloudapp.azure.com:/data/dome5132fileshare/DeepLearningWithAudio/utilities/dlwa/inputs/your_name 
```

* Transfering a file
```
scp -P 63635 input_name.wav e5132-admin@ml-lab-00cec95c-0f8d-40ef-96bb-8837822e93b6.westeurope.cloudapp.azure.com:/data/dome5132fileshare/DeepLearningWithAudio/utilities/dlwa/inputs/your_name
```
Please note that the text **"63635"** in the command line above should be changed with your personal info. You can find it in the ssh command line in the pop up connect window. (see the  [login instructions](https://github.com/SopiMlab/DeepLearningWithAudio/blob/master/00_introduction/))

**input_folder** and should be replaced with your directory path in your own machine as well as the folder **your_name**. Please note that the name you give to **input_folder** will be used in below command lines as well.



## Preparing your dataset

```
./dlwa.py ddsp make-dataset --input_name your_name/input_folder --dataset_name your_name/dataset_folder 
```
**your_name/input_folder** and  **your_name/dataset_folder** should be replaced with your own folder names. Saves data.tfrecord files into DeepLearningWithAudio/utilities/dlwa/dataset/ddsp/your_name/dataset_folder 



## Starting the training

```
./dlwa.py ddsp train --dataset_name your_name/dataset_folder --model_name your_name/name_model
```
**your_name/dataset_folder** and  **your_name/name_model** should be replaced with your own folder names. This command line will  start the DDSP training and it will  saves trained checkpoints, log, summaries into DeepLearningWithAudio/utilities/dlwa/models/ddsp/your_name/name_model



### Monitor the training

It is most likely that DDSP training will take approximatley 17 hours, during which you can log in and monitor the status of your training. To do that;

Log in to  https://labs.azure.com
(see the  [login instructions](https://github.com/SopiMlab/DeepLearningWithAudio/blob/master/00_introduction/))

c/p the command line below into your ternimnal window to go to the dlwa directory

```
cd /data/dome5132fileshare/DeepLearningWithAudio/utilities/dlwa
./dlwa.py util screen-attach
```

If your **traning still continues**, you will see similar output on your termninal window :
```
I0413 06:28:29.788176 140451635803968 train_util.py:306] step: 25825    spectral_loss: 5.92     total_loss: 5.92  
I0413 06:28:31.960153 140451635803968 train_util.py:306] step: 25826    spectral_loss: 5.85     total_loss: 5.85  
I0413 06:28:34.149478 140451635803968 train_util.py:306] step: 25827    spectral_loss: 5.64     total_loss: 5.64  
I0413 06:28:36.336162 140451635803968 train_util.py:306] step: 25828    spectral_loss: 4.78     total_loss: 4.78 
```

If your **traning is completed**, you will see the below text on your terminal window :

```
script failed: attach dlwa screen
aborting
```


## Transfering your trained model to your own computer/laptop

You can transfer your files, such as trained models from your the virtual machine to your on own PC  following the below command line structure. Open a new terminal window make sure that you are in your own computer/laptop directory.

* Transfering a folder:

```
scp -P 63635 -r e5132-admin@ml-lab-00cec95c-0f8d-40ef-96bb-8837822e93b6.westeurope.cloudapp.azure.com:/data/dome5132fileshare/DeepLearningWithAudio/utilities/dlwa/models/ddsp/your_name/mysound ~/Downloads
```

Please note that the text **"63635"** in the command line above should be changed with your personal info. You can find it in the ssh command line in the pop up connect window. (see the  [login instructions](https://github.com/SopiMlab/DeepLearningWithAudio/blob/master/00_introduction/))

**your_name/mysound** and should be replaced with your directory path in your own machine. 