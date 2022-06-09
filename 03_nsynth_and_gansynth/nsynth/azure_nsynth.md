# Generating Audio with NSynth in AzureVM

Log in to  https://labs.azure.com
(see the  [login instructions](https://github.com/SopiMlab/DeepLearningWithAudio/blob/master/00_introduction/))

c/p the command line below into your ternimnal window to go to the dlwa directory

```
cd /data/dome5132fileshare/DeepLearningWithAudio/utilities/dlwa
```



## Transfering your dataset to the virtual machine

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

**input_folder** and should be replaced with your directory path in your own machine as well as the folder **your_name**. 
Please note that the name you give to **input_folder** will be used in below command lines as well.



## Preparing your dataset

```
./dlwa.py nsynth prepare --input_name your_name/nsynth --output_name your_name/nsynth
```
**your_name/nsynth** and  **your_name/nsynth** should be replaced with your own folder names. Saves the files into DeepLearningWithAudio/utilities/dlwa/inputs/your_name/nsynth (It will create the folder **your_name/nsynth**, don't need to create it before)


## Starting generating Audio Samples

```
./dlwa.py nsynth generate --input_name your_name/nsynth --output_name your_name/nsynth --gpu 1
```
**your_name/nsynth** and  **your_name/nsynth** should be replaced with your own folder names. 
This command line will start generating the audio samples and and it will save trained checkpoints into DeepLearningWithAudio/utilities/dlwa/models/nsynth/**your_name/nsynth** folder


### Monitor the training

It is most likely that GANSynth training will take approximatley 48 hours, during which you can log in and monitor the status of your training. To do that;

Log in to  https://labs.azure.com
(see the  [login instructions](https://github.com/SopiMlab/DeepLearningWithAudio/blob/master/00_introduction/))

c/p the command line below into your ternimnal window to go to the dlwa directory

```
cd /data/dome5132fileshare/DeepLearningWithAudio/utilities/dlwa
./dlwa.py util screen-attach
```

If your **audio generation still continues**, you will see similar output on your termninal window :

```
/data/dome5132fileshare/DeepLearningWithAudio/utilities/dlwa/outputs/nsynth/chouteau/nsynth/workdir/audio_output/batch0/gen_keyboardelectronic_0.047_organelectronic_0.596_pitch_60_reedacoustic_0.357.wav
I0413 13:17:32.301989 139917840717632 fastgen.py:175] Saving: /data/dome5132fileshare/DeepLearningWithAudio/utilities/dlwa/outputs/nsynth/chouteau/nsynth/workdir/audio_output/batch0/gen_keyboardelectronic_0.047_organelectronic_0.596_pitch_60_reedacoustic_0.357.wav
INFO:tensorflow:Saving: /data/dome5132fileshare/DeepLearningWithAudio/utilities/dlwa/outputs/nsynth/chouteau/nsynth/workdir/audio_output/batch0/gen_keyboardelectronic_0.047_organelectronic_0.596_pitch_64_reedacoustic_0.357.wav
I0413 13:17:33.942848 139917840717632 fastgen.py:175] Saving: /data/dome5132fileshare/DeepLearningWithAudio/utilities/dlwa/outputs/nsynth/chouteau/nsynth/workdir/audio_output/batch0/gen_keyboardelectronic_0.047_organelectronic_0.596_pitch_64_reedacoustic_0.357.wav
```

If your **audio generation is completed**, you will see the below text on your terminal window :

```
script failed: attach dlwa screen
aborting
```


## Transfering your trained model to your own computer/laptop

You can transfer your files, such as trained models from your the virtual machine to your on own PC  following the below command line structure. Open a new terminal window make sure that you are in your own computer/laptop directory.

* Transfering a folder

```
scp -P 63635 -r e5132-admin@ml-lab-00cec95c-0f8d-40ef-96bb-8837822e93b6.westeurope.cloudapp.azure.com:/data/dome5132fileshareDeepLearningWithAudio/utilities/dlwa/models/nsynth/your_name/nsynth ~/Downloads
```

Please note that the text **"63635"** in the command line above should be changed with your personal info. You can find it in the ssh command line in the pop up connect window. (see the  [login instructions](https://github.com/SopiMlab/DeepLearningWithAudio/blob/master/00_introduction/))

**your_name/nsynth** and should be replaced with your directory path in your own machine. 