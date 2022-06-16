# DDSP Training on AzureVM

This guide is based on the DLWA script that aims to simplify usage of the models studied in the DeepLearningWithAudio course.  
For more information on how to use it, and on the organization of the directory, please take a look [here](../../../utilities/dlwa).

---

Log in to https://labs.azure.com
(see the [login instructions](https://github.com/SopiMlab/DeepLearningWithAudio/blob/master/00_introduction/))

Enter the DLWA directory:
```
cd /data/dome5132fileshare/DeepLearningWithAudio/utilities/dlwa
```

## Create a dataset

### Transfer your files to the VM 

You can transfer your files from your own PC to the VM following the below command line structure. 
Open a new terminal window and make sure that you are in your own computer/laptop directory.

* Transfer a folder

Let's assume that the folder you want to transfer is called: `violin`. The command line will be:

```
scp -P 63635 -r violin e5132-admin@ml-lab-00cec95c-0f8d-40ef-96bb-8837822e93b6.westeurope.cloudapp.azure.com:/data/dome5132fileshare/DeepLearningWithAudio/utilities/dlwa/inputs/your_name 
```

* Transfer a file

To transfert just a file, it is the same command line without the ```-r``` (-r = recursive).  
For example, if you want to transfer a file that is called: `violin.wav`, the command will be:
```
scp -P 63635 violin.wav e5132-admin@ml-lab-00cec95c-0f8d-40ef-96bb-8837822e93b6.westeurope.cloudapp.azure.com:/data/dome5132fileshare/DeepLearningWithAudio/utilities/dlwa/inputs/your_name
```

Note:
- The number (*63635*) and *your_name* in the command line above should be changed with your personal info. 
You can find your own number in the ssh command line that you use to connect to the VM. (see the [login instructions](../../../00_introduction/))



## Preparing your dataset

```
./dlwa.py ddsp make-dataset --input_name your_name/violin --dataset_name your_name/myviolindataset 
```

Saves data.tfrecord files in the `dataset/ddsp/your_name/myviolindataset` directory.

Note:
- *your_name/violin* and  *your_name/myviolindataset* should be replaced with your own folder names.



## Starting the training

```
./dlwa.py ddsp train --dataset_name your_name/myviolindataset --model_name your_name/myviolinmodel
```

This command line will start the DDSP training and saves trained checkpoints, log, summaries in the `models/ddsp/your_name/myviolinmodel` directory.

Note:
- *your_name/myviolindataset* and  *your_name/myviolinmodel* should be replaced with your own folder names.



### Monitor the training

It is most likely that DDSP training will take approximatley 17 hours, during which you can log in and monitor the status of your training. To do that;

Log in to https://labs.azure.com
(see the [login instructions](https://github.com/SopiMlab/DeepLearningWithAudio/blob/master/00_introduction/))

Enter the DLWA directory:
```
cd /data/dome5132fileshare/DeepLearningWithAudio/utilities/dlwa
./dlwa.py util screen-attach
```

- If your **traning still continues**, you will see similar output on your termninal window:
```
I0413 06:28:29.788176 140451635803968 train_util.py:306] step: 25825    spectral_loss: 5.92     total_loss: 5.92  
I0413 06:28:31.960153 140451635803968 train_util.py:306] step: 25826    spectral_loss: 5.85     total_loss: 5.85  
I0413 06:28:34.149478 140451635803968 train_util.py:306] step: 25827    spectral_loss: 5.64     total_loss: 5.64  
I0413 06:28:36.336162 140451635803968 train_util.py:306] step: 25828    spectral_loss: 4.78     total_loss: 4.78 
```

- If your **traning is completed or ended with an error**, you will see the below text:
```
script failed: attach dlwa screen
aborting
```


## Transfer your trained model to your own laptop

You can transfer your files, such as trained models from your the virtual machine to your on own PC  following the below command line structure. 
Open a new terminal window make sure that you are in your own computer/laptop directory.  

* Transfer a folder:

```
scp -P 63635 -r e5132-admin@ml-lab-00cec95c-0f8d-40ef-96bb-8837822e93b6.westeurope.cloudapp.azure.com:/data/dome5132fileshare/DeepLearningWithAudio/utilities/dlwa/models/ddsp/your_name/myviolinmodel ~/Downloads
```

Note:  
- The number (*63635*) in the command line above should be changed with your personal info.  
You can find your own number in the ssh command line that you use to connect to the VM. (see the  [login instructions](../../../00_introduction/))
- *your_name/myviolinmodel* and *~/Downloads* should be replaced with your directory path in your own machine. 