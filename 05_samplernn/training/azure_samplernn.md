## SampleRNN Training in AzureVM

Log in to  https://labs.azure.com
(see the  [login instructions](https://github.com/SopiMlab/DeepLearningWithAudio/blob/master/00_introduction/))

c/p the command line below into your ternimnal window to go to the dlwa directory

```
cd /data/dome5132fileshare/DeepLearningWithAudio/utilities/dlwa
```



### Transfering your dataset to the virtual machine

Please  before transferring your files and running chunk_audio, you should convert your audio to your desired sample rate (16000 Hz by default) and mono. The train script is supposed to handle this automatically, but it seems to be buggy at the moment.

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



### Preparing your dataset

```
./dlwa.py samplernn chunk-audio --input_name your_name/myinputs --output_name your_name/myinputs_chunks
```
**your_name/myinputs** and  **your_name/myinputs_chunks** should be replaced with your own folder names. Saves chopped files into DeepLearningWithAudio/utilities/dlwa/inputs/your_name/myinputs_chunk (It will create the folder **myinputs_chun**, don't need to create it before)



### Starting the training

```
./dlwa.py samplernn train --input_name your_name/myinputs_chunks --model_name  your_name/model_name  --preset lstm-linear-skip
```
**your_name/myinputs_chunks** and  **your_name/model_name** should be replaced with your own folder names. This command line will start the SampleRNN training and it will save the trained checkpoints and logs into DeepLearningWithAudio/utilities/dlwa/models/samplernn/**your_name/model_name** and it will save the generated audio into DeepLearningWithAudio/utilities/dlwa/generated/**your_name/model_name**. Please also note that --preset lstm-linear-skip is the default choice with dlwa script.



#### Monitor the training

It is most likely that SampleRNN training will take approximatley 38 hours, during which you can log in and monitor the status of your training. To do that;

Log in to  https://labs.azure.com
(see the  [login instructions](https://github.com/SopiMlab/DeepLearningWithAudio/blob/master/00_introduction/))

c/p the command line below into your ternimnal window to go to the dlwa directory

```
cd /data/dome5132fileshare/DeepLearningWithAudio/utilities/dlwa
./dlwa.py util screen-attach
```

If your **traning still continues**, you will see similar output on your termninal window :

```
Epoch: 27/100, Step: 6/250, Loss: 1.355, Accuracy: 46.000, (4.365 sec/step)
Epoch: 27/100, Step: 7/250, Loss: 1.352, Accuracy: 46.179, (4.377 sec/step)
Epoch: 27/100, Step: 8/250, Loss: 1.349, Accuracy: 46.208, (4.323 sec/step)
Epoch: 27/100, Step: 9/250, Loss: 1.344, Accuracy: 46.309, (4.358 sec/step)
```

If your **traning is completed**, you will see the below text on your terminal window :

```
script failed: attach dlwa screen
aborting! 
```


### Transfering your trained model to your own computer/laptop

You can transfer your files, such as trained models from your the virtual machine to your on own PC  following the below command line structure. Open a new terminal window make sure that you are in your own computer/laptop directory.

* Transfering a folder

```
scp -P 63635 -r e5132-admin@ml-lab-00cec95c-0f8d-40ef-96bb-8837822e93b6.westeurope.cloudapp.azure.com:/data/dome5132fileshareDeepLearningWithAudio/utilities/dlwa/models/samplernn/your_name/model_name ~/Downloads
```

Please note that the text **"63635"** in the command line above should be changed with your personal info. You can find it in the ssh command line in the pop up connect window. (see the  [login instructions](https://github.com/SopiMlab/DeepLearningWithAudio/blob/master/00_introduction/))

**your_name/mysound** and should be replaced with your directory path in your own machine.