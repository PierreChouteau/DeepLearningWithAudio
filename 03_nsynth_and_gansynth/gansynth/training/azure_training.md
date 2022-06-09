## GANSynth Training in Azure My Virtual Machines

Log in to  https://labs.azure.com
(see the  [login instructions](https://github.com/SopiMlab/DeepLearningWithAudio/blob/master/00_introduction/))

c/p the command line below into your terminal window to go to the dlwa directory:

```
cd /data/dome5132fileshare/DeepLearningWithAudio/utilities/dlwa
```


### Transfering your dataset to the virtual machine

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
./dlwa.py gansynth chop-audio --input_name your_name/input_folder_to_be_transferred --output_name your_name/mysounds_chopped
```

**your_name/input_folder** and  **your_name/mysounds_chopped** should be replaced with your own folder names. Saves chopped files into DeepLearningWithAudio/utilities/dlwa/inputs/mysounds_chopped (It will create the folder **mysounds_chopped**, don't need to create it before)

```
./dlwa.py gansynth make-dataset --input_name your_name/mysounds_chopped --dataset_name your_name/mysounds 
```

**your_name/mysounds_chopped** and  **your_name/mysounds** should be replaced with your own folder names. Saves data.tfrecord files into DeepLearningWithAudio/utilities/dlwa/datasets/gansynth/**your_name/mysounds**



### Starting the training

```
./dlwa.py gansynth train --dataset_name your_name/mysounds --model_name your_name/model
```

**your_name/mysounds** and  **your_name/model** should be replaced with your own folder names. This command line will start the GANSynth training and and it will save trained checkpoints into DeepLearningWithAudio/utilities/dlwa/models/gansynth/**your_name/mysound** folder


#### Monitor the training

It is most likely that GANSynth training will take approximatley 48 hours, during which you can log in and monitor the status of your training. To do that;

Log in to  https://labs.azure.com
(see the  [login instructions](https://github.com/SopiMlab/DeepLearningWithAudio/blob/master/00_introduction/))

c/p the command line below into your ternimnal window to go to the dlwa directory

```
cd /data/dome5132fileshare/DeepLearningWithAudio/utilities/dlwa
./dlwa.py util screen-attach
```

If your **traning still continues**, you will see similar output on your termninal window :
```
I0409 16:32:56.410543 140269805838720 basic_session_run_hooks.py:260] Tensor("status_message:0", shape=(), dtype=string) = b'Starting train step: current_image_id: 184, progress: 0.000000, num_blocks: 1, batch_size: 8' (0.123 sec)
INFO:tensorflow:Tensor("status_message:0", shape=(), dtype=string) = b'Starting train step: current_image_id: 192, progress: 0.000000, num_blocks: 1, batch_size: 8' (0.105 sec)
I0409 16:32:56.515631 140269805838720 basic_session_run_hooks.py:260] Tensor("status_message:0", shape=(), dtype=string) = b'Starting train step: current_image_id: 192, progress: 0.000000, num_blocks: 1, batch_size: 8' (0.105 sec)
INFO:tensorflow:Tensor("status_message:0", shape=(), dtype=string) = b'Starting train step: current_image_id: 200, progress: 0.000000, num_blocks: 1, batch_size: 8' (0.112 sec)
I0409 16:32:56.627448 140269805838720 basic_session_run_hooks.py:260] Tensor("status_message:0", shape=(), dtype=string) = b'Starting train step: current_image_id: 200, progress: 0.000000, num_blocks: 1, batch_size: 8' (0.112 sec)
INFO:tensorflow:Tensor("status_message:0", shape=(), dtype=string) = b'Starting train step: current_image_id: 208, progress: 0.000000, num_blocks: 1, batch_size: 8' (0.112 sec)
I0409 16:32:56.739801 140269805838720 basic_session_run_hooks.py:260] Tensor("status_message:0", shape=(), dtype=string) = b'Starting train step: current_image_id: 208, progress: 0.000000, num_blocks: 1, batch_size: 8' (0.112 sec) 
```

If your **traning is completed**, you will see the below text on your terminal window :
```
script failed: attach dlwa screen
aborting
```

### Transfering your trained model to your own computer/laptop

You can transfer your files, such as trained models from your the virtual machine to your on own PC  following the below command line structure. Open a new terminal window make sure that you are in your own computer/laptop directory.

* Transfering a folder

```
scp -P 63635 -r e5132-admin@ml-lab-00cec95c-0f8d-40ef-96bb-8837822e93b6.westeurope.cloudapp.azure.com:/data/dome5132fileshareDeepLearningWithAudio/utilities/dlwa/models/gansynth/your_name/mysound ~/Downloads
```

Please note that the text **"63635"** in the command line above should be changed with your personal info. You can find it in the ssh command line in the pop up connect window. (see the  [login instructions](https://github.com/SopiMlab/DeepLearningWithAudio/blob/master/00_introduction/))

**your_name/mysound** and should be replaced with your directory path in your own machine. 