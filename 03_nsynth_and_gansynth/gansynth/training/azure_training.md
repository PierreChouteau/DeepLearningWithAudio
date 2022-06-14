# GANSynth Training in Azure VM

Log in to https://labs.azure.com
(see the  [login instructions](https://github.com/SopiMlab/DeepLearningWithAudio/blob/master/00_introduction/))

Enter the DLWA directory:
```
cd /data/dome5132fileshare/DeepLearningWithAudio/utilities/dlwa
```

## Create a dataset

### Transfer your files to the VM 

You can transfer your files from your own PC to the VM following the below command line structure. 
Open a new terminal window and make sure that you are in your own computer/laptop directory.

* Transfer a folder

Let's assume that the folder you want to transfer is called: `mytunes`. The command line will be:

```
scp -P 63635 -r mytunes e5132-admin@ml-lab-00cec95c-0f8d-40ef-96bb-8837822e93b6.westeurope.cloudapp.azure.com:/data/dome5132fileshare/DeepLearningWithAudio/utilities/dlwa/inputs/your_name 
```

* Transfer a file

To transfert just a file, it is the same command line without the ```-r```. (-r = recursive).
For example, if you want to transfer a file that is called: `myfile.wav`, the command will be:
```
scp -P 63635 input_name.wav e5132-admin@ml-lab-00cec95c-0f8d-40ef-96bb-8837822e93b6.westeurope.cloudapp.azure.com:/data/dome5132fileshare/DeepLearningWithAudio/utilities/dlwa/inputs/your_name
```

Note: 
* The number (**63635**) and **your_name** in the command line above should be changed with your personal info. 
You can find your own number in the ssh command line that you use to connect to the VM. (see the [login instructions](https://github.com/SopiMlab/DeepLearningWithAudio/blob/master/00_introduction/))


### Preparing your dataset

```
./dlwa.py gansynth chop-audio --input_name your_name/mytunes --output_name your_name/mysounds_chopped
```

**your_name/input_folder** and  **your_name/mysounds_chopped** should be replaced with your own folder names. Saves chopped files into DeepLearningWithAudio/utilities/dlwa/inputs/mysounds_chopped (It will create the folder **mysounds_chopped**, don't need to create it before)


### Convert to TFRecord

GANSynth expects input in the TFRecord format (a generic file format for TensorFlow data), so the WAV files need to be converted. This can be done with our script `make_dataset.py`.

Run it as follows:

```
./dlwa.py gansynth make-dataset --input_name your_name/mysounds_chopped --dataset_name your_name/mydataset 
```

This command line will look into the directory `inputs/your_name/mysounds_chopped` and generates 2 files (`data.tfrecord` and `meta.json`) in the output directory `datasets/gansynth/your_name/mydataset`

Note: 
* **your_name/mysounds_chopped** and  **your_name/mydataset** should be replaced with your own folder names. 


## Run the training

```
./dlwa.py gansynth train --dataset_name your_name/mydataset --model_name your_name/mymodel
```

This command line will start the GANSynth training. It will load the training data that is stored in the 2 files, `data.tfrecord` and `meta.json`, and generate checkpoints in the `gansynth/your_name/mymodel` directory

Note:
* **your_name/mydataset** and  **your_name/mymodel** should be replaced with your own folder names.


### Monitor the training

It is most likely that GANSynth training will take approximatley 48 hours, during which you can log in and monitor the status of your training. To do that :

Log in to  https://labs.azure.com
(see the  [login instructions](https://github.com/SopiMlab/DeepLearningWithAudio/blob/master/00_introduction/))


Enter the DLWA directory:
```
cd /data/dome5132fileshare/DeepLearningWithAudio/utilities/dlwa
./dlwa.py util screen-attach
```

* If your **traning still continues**, you will see similar output on your termninal window:
```
I0409 16:32:56.410543 140269805838720 basic_session_run_hooks.py:260] Tensor("status_message:0", shape=(), dtype=string) = b'Starting train step: current_image_id: 184, progress: 0.000000, num_blocks: 1, batch_size: 8' (0.123 sec)
INFO:tensorflow:Tensor("status_message:0", shape=(), dtype=string) = b'Starting train step: current_image_id: 192, progress: 0.000000, num_blocks: 1, batch_size: 8' (0.105 sec)
I0409 16:32:56.515631 140269805838720 basic_session_run_hooks.py:260] Tensor("status_message:0", shape=(), dtype=string) = b'Starting train step: current_image_id: 192, progress: 0.000000, num_blocks: 1, batch_size: 8' (0.105 sec)
INFO:tensorflow:Tensor("status_message:0", shape=(), dtype=string) = b'Starting train step: current_image_id: 200, progress: 0.000000, num_blocks: 1, batch_size: 8' (0.112 sec)
I0409 16:32:56.627448 140269805838720 basic_session_run_hooks.py:260] Tensor("status_message:0", shape=(), dtype=string) = b'Starting train step: current_image_id: 200, progress: 0.000000, num_blocks: 1, batch_size: 8' (0.112 sec)
INFO:tensorflow:Tensor("status_message:0", shape=(), dtype=string) = b'Starting train step: current_image_id: 208, progress: 0.000000, num_blocks: 1, batch_size: 8' (0.112 sec)
I0409 16:32:56.739801 140269805838720 basic_session_run_hooks.py:260] Tensor("status_message:0", shape=(), dtype=string) = b'Starting train step: current_image_id: 208, progress: 0.000000, num_blocks: 1, batch_size: 8' (0.112 sec) 
```

* If your **traning is completed or ended with an error**, you will see the below text on your terminal window:
```
script failed: attach dlwa screen
aborting
```

## Transfer your trained model to your own laptop

You can transfer your files, such as trained models from your the virtual machine to your on own PC  following the below command line structure. 
Open a new terminal window make sure that you are in your own computer/laptop directory.

* Transfer a folder
```
scp -P 63635 -r e5132-admin@ml-lab-00cec95c-0f8d-40ef-96bb-8837822e93b6.westeurope.cloudapp.azure.com:/data/dome5132fileshareDeepLearningWithAudio/utilities/dlwa/models/gansynth/your_name/mymodel ~/Downloads
```

Note:
* The number (**63635**) in the command line above should be changed with your personal info. 
You can find your own number in the ssh command line that you use to connect to the VM. (see the  [login instructions](https://github.com/SopiMlab/DeepLearningWithAudio/blob/master/00_introduction/))

* **your_name/mymodel** and should be replaced with your directory path in your own machine. 
