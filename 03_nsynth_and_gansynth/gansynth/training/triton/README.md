# Training GANSynth on Triton

This guide supplements the [main training guide](../README.md). It is assumed you have access to the [Triton](https://scicomp.aalto.fi/triton/) computing cluster and basic familiarity with how to run batch jobs there.

## Set up the Conda environment

```
cd "$WRKDIR"
git clone https://github.com/SopiMlab/magenta.git
git clone https://github.com/SopiMlab/DeepLearningWithAudio.git
module -q load anaconda3
mkdir -p "$WRKDIR/conda"
conda env create -p "/PATH/TO/DIR/conda/gansynth" --file="$WRKDIR/DeepLearningWithAudio/03_nsynth_and_gansynth/gansynth/training/gansynth-training-env.yml"

source activate "$WRKDIR/conda/gansynth"
cd magenta
pip install -e .
```

### protobuf workaround

```
cd $WRKDIR/DeepLearningWithAudio/03_nsynth_and_gansynth/gansynth/training
mkdir repos 
cd repos
git clone --branch v3.13.0 https://github.com/protocolbuffers/protobuf.git
cd protobuf
git submodule update --init --recursive
./autogen.sh
cd python
python setup.py build
conda remove --yes --force protobuf libprotobuf
python setup.py develop
```

## Train

### GANSynth Training

Prepare your dataset and copy the files to Triton, then submit a batch job using our `train.slrm` script. The script requires the following arguments:

- Path to the Conda environment
- Path to your training data file (`data.tfrecord`)
- Path to your training metadata file (`meta.json`)
- Path to the directory in which to save the trained model

It is best to submit the job from a fresh shell with no modules loaded and no Conda environment active, as an active environment may [mess up](https://version.aalto.fi/gitlab/AaltoScienceIT/triton/issues/612) some Python paths.

```
cd "$WRKDIR/DeepLearningWithAudio/03_nsynth_and_gansynth/gansynth/training/triton"
```

To submit a GANSynth training job, run:

```
sbatch train.slrm \
    --conda_env "$WRKDIR/conda/gansynth" \
    --train_data_path "$WRKDIR/mydataset/data.tfrecord" \
    --train_meta_path "$WRKDIR/mydataset/meta.json" \
    --train_root_dir "$WRKDIR/mymodel"
```

If you want to change the config file for training parameters, like batch size, num_trans_images... just go to : 

```
cd $WRKDIR/magenta/magenta/models/gansynth/configs
vim mel_prog_hires.py
```

Change what you want (to change something on vim, you need to be in --insert-- mode ! Type "i" to enter this mode. 
To save your changes it is ":x" | to quit ":q" | to quit without save changes ":q!" 


To train with a dataset that has extra labels (here `nsynth_qualities_tfrecord`):

```
sbatch train.slrm \
    --conda_env "$WRKDIR/conda/gansynth" \
    --train_data_path "$WRKDIR/mydataset/data.tfrecord" \
    --train_meta_path "$WRKDIR/mydataset/meta.json" \
    --train_root_dir "$WRKDIR/mymodel" \ 
    --dataset_name nsynth_qualities_tfrecord
```

### PCA for GANSpaceSynth

You can submit a batch job using our `ganspace.slrm` script. The script requires the following arguments:

- Path to the Conda environment
- Path to the directory in which is located your GANSynth model


To compute PCA for GANSpaceSynth, run:

batch ganspace.slrm \
    --conda_env "$WRKDIR/conda/gansynth" \
    --ckpt_dir "$WRKDIR/mymodel"